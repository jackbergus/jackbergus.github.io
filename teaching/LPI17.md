---
layout: post
title: Web Programming Lab 2017 – The Lab Sessions
tags: teaching
---

Main Lecturer: prof. Ferretti

* TOC
{:toc}

## [Lab Sessions, 2017](#2017)



*  2017/10/10: *Introductory Lesson* [1,5H x2]
   * [How to use your lab? (IT)](https://github.com/jackbergus/LPI07/raw/master/Lesson00/LabRuleBook.pdf)
   * [Programs](https://github.com/jackbergus/LPI07/tree/master/Lesson00) for the zero lesson, and some [introductory slides](https://github.com/jackbergus/LPI07/raw/master/Lesson00/ex00.pdf).
* 2017/10/24: *Native types, casts, String, Scanner from System.in, Math* [1,5H x2]
   * Exercises [[EN](https://github.com/jackbergus/LPI07/raw/master/Lesson01/guide.pdf),[IT](https://github.com/jackbergus/LPI07/raw/master/Lesson01/guide_it.pdf)] and the [proposed solutions](https://github.com/jackbergus/LPI07/tree/master/Lesson01).

### Additional advanced notes

 * 2017/10/10: *Introductory Lesson*
     * [Introduction to Compilers, Interpreters, and Programs (IT)](https://github.com/jackbergus/LPI07/raw/master/Lesson00/00Compilers.pdf)
 * 2017/10/24: 
     * [What is integer overflow?](https://en.wikipedia.org/wiki/Integer_overflow)
     * [What is parsing? (IT)](https://it.wikipedia.org/wiki/Parsing)
     * What is a [pairing function](http://www.cs.upc.edu/~alvarez/calculabilitat/enumerabilitat.pdf) using the [dovetailing](https://en.wikipedia.org/wiki/Dovetailing_(computer_science)) technique?
 * 2017/??/??: *Designing Classes*
     * An example of [state driven programming](https://github.com/jackbergus/LucenePdfIndexer) (CSS theory is required)

Main References:
* Walter Savitch. "**JAVA: An introduction to Problem Solving and Programming, 6th edition**". 2012, Pearson Education.

Further references:
* M. Felleisen et al. ["**How To Design Classes**"](http://www.ccs.neu.edu/home/matthias/HtDC/htdc.pdf)
* H. Schildt. "**Java: The Complete Reference, Ninth Edition**" 

# FAQ (IT)

#### Come faccio ad installare un programma?
Molto probabilmente, la tua distribuzione Linux avrà solo `gedit` o `pluma` installati di default. Per installare gli altri editor, eseguire il comando di installazione: 
 * `sudo apt-get install nomeprogramma`. Questo comando è eseguibile direttamente in alcune distribuzioni, quali Ubuntu, dove il tuo utente predifinito è già previsto come *[sudoer](https://wiki.archlinux.org/index.php/Sudo_(Italiano))*.
 * Per altri sistemi operativi, si può o [aggiungere l'utente corrente](https://ubuntuforums.org/showthread.php?t=1132821) alla lista dei sudoer e poi proseguire con il comando di cui sopra, o diventare prima superutente con `su -` o `sudo su -`. 

#### Come creare un file sorgente Java?
Come editor di testo, le macchine di laboratorio supportano gedit, jedit, geany. Conseguentemente, per editare il codice sorgente che da genererà il programma (bytecode), può eseguire da terminale i seguenti comandi (in grassetto):
 * `gedit File.java`
 * `jedit File.java`
 * `geany File.java`

Tuttavia, il file non è creato: esso verrà creato con il nome specificato solo dopo aver effettuato il salvataggio. Si può usare anche il comando `touch` visto a lezione per generare un nuovo file vuoto.

#### Come compilare un file sorgente Java?
Dopo aver installato la Java JDK e JRE, "java si trova sotto forma" di due programmi, il compilatore `javac` e l'interprete `java`. In particolare, per compilare il programma è opportuno si deve eseguire il seguente comando `javac File.java`. In particolare, il compilatore  prende in input il file scritto in linguaggio java (.java) e produce il bytecode (.class) con il nome della classe dichiarata al suointero.

#### Come eseguire un programma Java?
In questo corso vedremo solamente la creazione di programmi nonsituati all'interno di un `package`. Per eseguire un `main` di classe non contenuta in un package, è sufficiente indcare all'interprete (`java`) la classe da eseguire contenuta nella directory corrente nel seguente modo: `java NomeClasse`.

Nel caso in cui la classe sia contenuta all'interno del package, bisognerà specificare il percorso completo della stessa classe all'interno del package: `java full.package.path.NomeClasse`.

#### Sul mio computer l'editor di testo ha una visualizzazione differente rispetto a quella del computer del laboratorio. Come posso fare?
L'interfaccia grafica in sè non è un problema. Differenti visualizzazioni possono essere dovute a differenti sistemi operativi, a differenti versioni del pacchetto installato o a differenti [window manager](https://it.wikipedia.org/wiki/Window_manager). L'unico scopo di un editor di testo è quello di produrre del testo, e di avere una comoda [colorazione della sintassi](https://it.wikipedia.org/wiki/Syntax_highlighting).

![Prima visualizzazione di Gedit (GNU/Linux)](http://1.bp.blogspot.com/-5uhn4nSKk2I/UtbMnYOJHyI/AAAAAAABRiA/ysLOK2Di0w8/s1600/gedit4.png)
![Seconda visualizzazione di Gedit (GNU/Linux)](https://d2.alternativeto.net/dist/s/5d440bae-e4cc-466b-861b-f4c6ec293baa_1_full.png?format=jpg&width=1600&height=1600&mode=min&upscale=falseg)
![Gedit su Windows.](https://cdn.forumer.it/gen_screenshots/it-IT/windows/gedit/large/gedit-02-657x535.png)
