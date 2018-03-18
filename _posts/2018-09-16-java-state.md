---
layout: post
title: Process algebra and StateCharts
tags: java programming, statecharts, CSS
---

If we want to model an access to a resource that could not be read and written at the same time, we could use either process algebra or State Charts from UML analysis. In either cases the desired object is an element that could perform both read and write operations, without changing the type of the element or to remember to close and re-open the object in a different state. 

     P = read.P + write.P
     
![P = read.P + write.P](https://raw.githubusercontent.com/jackbergus/LucenePdfIndexer/master/images/Diagramma1.png)

We know that usually it is not good to keep reading and writing operations on the same object, and hence we have always to close and re-open the object if we want to change the type of the operation. As a consequence we have to handle three objects at a time, an initial state (```P```), an opened state for reading purposes (```R```) and another one for writing (```W```). This could be easily modelled as follows

    P = openRead.R + openWrite.W
    R = read.R + close.P
    W = write.W + close.P
    
![Representing states P, R and W](https://raw.githubusercontent.com/jackbergus/LucenePdfIndexer/master/images/twostates.png)

Now we want to hide all the open and close operations, and hence we want to define an object, that is a State Machine, that keeps track of all the object states and then "smartly decides" which operations have to be carried out in order to reach the desired operation to execute.

     ClosedObject = OpenRead._close.ClosedObject + OpenWrite._close.ClosedObject
     ReadableObject = _OpenRead.R
     WritableObject = _OpenWrite.W
     R = read.R + close.ReadableObject
     W = write.W + close.WritableObject
     
     (v (OpenRead,OpenWrite,close)) ( ClosedObject | ReadableObject | WritableObjeect)

![Representing the synchronized states](https://raw.githubusercontent.com/jackbergus/LucenePdfIndexer/master/images/complete.png)

We could easily see that this implementation corresponds to the first tone, that is the model that we want to reach.


## Using Interfaces to implement the internal states

Firstly, we must declare in Java that closed, opened, readable and writable states belong to a same type of object. This allows define an unique interface for any possible state of the internal object.

```java
/**
 * Defines a document that could be opened, closed, read and written.
 */
public interface ReadWriteDocument<T> {
}
```

As a next step we could define a closed object, that could only perform some open actions towards either the readable or the writable object. An ErrOptional object returns either a value or an error, that could be a Throwable object or a simple (error) Message as a string. 

```java
/**
 * This is the default state when an object is opened. It is neither ready to be read, nor ready to be written.
 * In order to perform a read or a write action, I have first to perform an open action
 */
public interface Closed<T> extends ReadWriteDocument<T> {
    public ErrOptional<DoRead<T>> openRead();
    public ErrOptional<DoWrite<T>> openWrite();
}
```

Consequently the writable object could be implemented as follows:

```java
public interface DoWrite<T> extends Opened<T> {
    public int writeObject(T obj) throws IOException;
}
```

Readable objects could be implemented in a similar fashon. If the underlying implementation
allows to read and write at the same time, we could even define an interface that allows to 
perform both operations as follows:

```java
public interface DoReadWrite<T> extends DoRead<T>, DoWrite<T> { }
```

## Implementing the interfaces for Lucene 5

At this point we define a simple class, that could be easily mapped into a Lucene Document. 

```java
public class LucenePaper extends LuceneDocument {

    public String text;
    public String bibtexkey;

    public LucenePaper(String text, String bibtexkey) {
        this.text = text;
        this.bibtexkey = bibtexkey;
    }

    public LucenePaper(Document doc) {
        if (doc == null) {
            text = bibtexkey = null;
        } else {
            this.text = doc.get("text");
            this.bibtexkey = doc.get("bibtexkey");
        }
    }
}
```

At this point we could implement the ```ClosedObject``` as the object that could perform some
outgoing operations into either a readable state or a writable one.

```java
public class ClosedLuceneIndex implements Closed<LucenePaper> {

    private final StandardAnalyzer analyzer;
    private FSDirectory index;
    private File directory;

    public ClosedLuceneIndex(File directory) {
        this.analyzer = new StandardAnalyzer();
        this.directory = directory;
        // Link the directory on the FileSystem to the application
        try {
            this.index = FSDirectory.open(directory.toPath());
        } catch (IOException e) {
            e.printStackTrace();
            index = null;
        }
    }

    public boolean exists() {
        try {
            return DirectoryReader.indexExists(index);
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * Creates the Write configuration as a Lucene object
     * @return
     */
    public ErrOptional<IndexWriter> writeConfiguration() {
        IndexWriterConfig config = new IndexWriterConfig(analyzer);
        try {
            return ErrOptional.of(new IndexWriter(index, config));
        } catch (IOException e) {
            return ErrOptional.raiseError(e);
        }
    }

    /**
     * Generates the Read configuration as a Lucene object
     * @return
     */
    public ErrOptional<IndexSearcher> openConfiguration() {
        try {
            IndexReader ir = DirectoryReader.open(index);
            IndexSearcher searcher = new IndexSearcher(ir);
            return ErrOptional.of(searcher);
        } catch (IOException e) {
            return ErrOptional.raiseError(e);
        }
    }

    /**
     * Generates the Readable state
     */
    @Override
    public ErrOptional<DoRead<LucenePaper>> openRead() {
        ErrOptional<IndexSearcher> wc = openConfiguration();
        if (wc.hasValue()) {
            DoRead<LucenePaper> d = new LuceneRead(directory,wc.get());
            return ErrOptional.of(d);
        } else {
            return wc.doCast();
        }
    }

    /**
     * Generates the Writable state
     */
    @Override
    public ErrOptional<DoWrite<LucenePaper>> openWrite() {
        ErrOptional<IndexWriter> wc = writeConfiguration();
        if (wc.hasValue()) {
            DoWrite<LucenePaper> d = new LuceneWrite(directory,wc.get());
            return ErrOptional.of(d);
        } else {
            return wc.doCast();
        }
    }
}
```

As a next step we could implement a ```LuceneRead``` class that closes the indices and returns a ```ClosedLuceneIndex``` and a ```LuceneWrite``` class that does the same thing for the write indices.

## Implementing the State Machine

Since Java7, we could define a class as implementing the ```AutoCloseable``` interface: this means that we know that any object is closed correctly after the ```try…catch``` clause as follows:

```java
try (LuceneStateMachine newObject = new LuceneStateMachine(file)) {
	//use the newObject
} catch (Exception e) {
	//do something
}
```

The State Machine could be initialized as starting in the closed state as follows:

```java
public class LuceneStateMachine implements AutoCloseable {

    private ReadWriteDocument<LucenePaper> state;

    public LuceneStateMachine(File file) {
        this.state = new ClosedLuceneIndex(file);
    }
}
```

We could check the closed state as follows:

```java
    /**
     * Closes the index through the state machine.
     */
    public ErrOptional<Boolean> stateClose() {
        ErrOptional<Closed<LucenePaper>> elem = ErrOptional.raiseMessageError("Error: unmatched case in closing element");

        if (state instanceof Closed)
            return ErrOptional.of(true);
        else if (state instanceof DoRead)
            elem = ((LuceneRead) state).close();
        else if (state instanceof DoWrite)
            elem = ((LuceneWrite) state).close();

        if (elem.hasValue()) {
            state = elem.get();
            return ErrOptional.of(true);
        } else {
            return elem.doCast();
        }
    }
    
    /**
     * Automatically closes the object
     * @throws Exception
     */
    @Override
    public void close() throws Exception {
        ErrOptional<Boolean> cs = stateClose();
        if (cs.isError()) {
            if (cs.isThrowable())
                cs.getError().printStackTrace();
            else if (cs.isMessage())
                throw new RuntimeException(cs.getMessage());
            throw new RuntimeException("HALT");
        }
    }
```

Now we could implement all the method that have been declared in the DoRead and DoWrite interfaces in the state machine, where each method has to perform the following methods in order to check if the machine is  on the correct state and, if not, to perform the state transaction to the correct one.

```java
private ErrOptional<Boolean> prepareRead() {
     if (!(state instanceof DoRead)) {
        ErrOptional<Boolean> l = stateClose();
        //returns the error if not correct execution
        if (!l.hasValue()) return l.doCast();
        if (l.get()==false) return ErrOptional.raiseMessageError("Error while closing the element");

        ErrOptional<DoRead<LucenePaper>> result = ((ClosedLuceneIndex) state).openRead();
        if (result.hasValue())
            state = result.get();
        else
            return result.doCast();
    }
    return ErrOptional.of(true);
}

private ErrOptional<Boolean> prepareWrite() {
    if (!(state instanceof DoWrite)) {
        ErrOptional<Boolean> l = stateClose();
        //returns the error if not correct execution
        if (!l.hasValue()) return l.doCast();
        if (l.get()==false) return ErrOptional.raiseMessageError("Error while closing the element");

        ErrOptional<DoWrite<LucenePaper>> result = ((ClosedLuceneIndex) state).openWrite();
        if (result.hasValue())
           state = result.get();
        else
           return result.doCast();
    }
    return ErrOptional.of(true);
}
```

As a final result, we could read and write a Lucene Index trivially as follows:

```java
//Ok: if there are no documents, there are no documents added yet
//In Java7, this statement ensures that the object will be closed at the end of the road
try (LuceneStateMachine state = new LuceneStateMachine(new File("/Users/vasistas/luceneindexstate"))) {
    if (state.getSize().isError()) {
        //add some papers
        state.write(new LucenePaper("this is some long text","key1"));
        state.write(new LucenePaper("this is some short text","key2"));
        System.out.println("Added some papers and closed");
    } else {
        //performing some read operations
        int size = get(state.getSize());
        System.out.println("Has size");
        System.out.println("size: "+size);

        Iterable<LucenePaper> it = get(state.asIterable());
        for (LucenePaper p : it) {
            System.out.println(p.text +" -- " + p.bibtexkey);
        }

        //performing a write operation
        size++;
        state.write(new LucenePaper("This is the new paper, number "+size,"key"+size));
        System.out.println("Added some new paper and closed");


    }
   } catch (Exception e) {
    e.printStackTrace();
   }
//Closing the object automatically at the end of the code block: Java7
```
