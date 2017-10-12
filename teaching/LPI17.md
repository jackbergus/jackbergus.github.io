---
layout: post
title: Web Programming Lab 2017 – The Lab Sessions
tags: teaching
---

Main Lecturer: prof. Ferretti

* TOC
{:toc}

## [Lab Sessions, 2017](#2017)



*  2017/10/10: *Introductory Lesson* [3H]
   * [How to use your lab? (IT)](https://github.com/jackbergus/LPI07/raw/master/Lesson00/LabRuleBook.pdf)
   * [Programs](https://github.com/jackbergus/LPI07/tree/master/Lession00) for the zero lesson, and some [introductory slides](https://github.com/jackbergus/LPI07/raw/master/Lesson00/ex00.pdf).
* 2017/10/24: *Main: Numerical Operations*
     * [TODO]

### Additional advanced notes

 * 2017/10/10: *Introductory Lesson*
     * [Introduction to Compilers, Interpreters, and Programs (IT)](https://github.com/jackbergus/LPI07/raw/master/Lesson00/00Compilers.pdf)
     
 * 2017/??/??: *Designing Classes*
   * An example of [state driven programming](https://github.com/jackbergus/LucenePdfIndexer) (CSS theory is required)

Main References:
* Walter Savitch. "**JAVA: An introduction to Problem Solving and Programming, 6th edition**". 2012, Pearson Education.

Further references:
* M. Felleisen et al. ["**How To Design Classes**"](http://www.ccs.neu.edu/home/matthias/HtDC/htdc.pdf)
* H. Schildt. "**Java: The Complete Reference, Ninth Edition**" 

# FAQ (IT)

#### Come create un file sorgente Java?
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
