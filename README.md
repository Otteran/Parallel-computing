# Parallel-computing
Log book of assignments and exercises of distributed systems and parallel computing

## Logbook Distributed systems and Parallel-computing


## Introduction
This git repository / logbook contains part of assignments and exercises done during 7 weeks course in parallel computing.

With more focus on OpenCL as the chosen topic to implement more functional programming language.

## Pre-requisites
(only if you want to run source codes in repository)

OpenCL 1.1 (or greater)

Xcode (Visual Studio if on Windows system)

C99 compiler (we use gcc) with OpenMP support (used for timing the runs [optional])
C++11 compiler (we use g++ or clang, also tested with Intel's icc)
Need help setting up OpenCL? Check out https://www.khronos.org/message_boards/showthread.php/6670-Help-setting-up-OpenCL for information about setting up OpenCL on Linux for AMD (CPU, GPU, APU), Intel CPUs and NVIDIA GPUs.

** 

# Week 1
### introduction to parallel computing (concurrency models)

## 7 Concurrency models
The 7 models that was introduced:

1. Threads and locks: the default choice for much concurrent software that has
many well-understood problems and often underpins other models.

2. Functional programming: increasingly prominent for many reasons, not the
least of which is its excellent support for concurrency and parallelism.

3. Identity/State: a particularly effective hybrid of imperative and functional
programming (Clojure).
4. Actor/Agent: a general-purpose concurrent programming model with wide
applicability to target both shared- and distributed-memory architectures.
5. Communicating Sequential Processes: CSP resembles the actor model but
puts emphasis on the channels used for communication.
6. Data parallelism: the GPU speeds up graphics processing, but it can used on
a much wider range of tasks: finite element analysis, computational fluid
dynamics, stellar simulations or anything else that involves significant number crunching,
7. The Lambda Architecture: combines the strengths of MapReduce and stream processing to solve Big Data problems.

### introduction to threads
(examples code and exercises/assignment done)
``1.	package com.paulbutcher;``

``2.	public class HelloWorld ``

``3.	{``

``4.	  public static void main(String[] args) throws InterruptedException ``

``5.	  {``

``6.	    Thread myThread = new Thread() ``

``7.	    {``

``8.	        public void run() ``

``9.	        {``

``10.	          System.out.println("Hello from new thread");``

``11.	        }``

``12.	      };``
``13.		  ``

``14.	    myThread.start();``

``15.	    Thread.yield();``

``19.	    System.out.println("Hello from main thread");``

``20.	    myThread.join();``

``21.	  }``
``22.	}``

two running threads, thread one displays 'Hello from new thread' and the second thread displays 'Hello from main thread'. after running the application 10 times, the second thread display first before the main thread.

### i. counting
`2.	public class Counting {`

`3.	  public static void main(String[] args) throws InterruptedException {`

`4.	    class Counter {`

`5.	      private int count = 0;`

`6.	      public void increment() { ++count; }`

`7.	      public int getCount() { return count; }`
`8.	    }`

`9.	    final Counter counter = new Counter();`

`10.	    class CountingThread extends Thread {`

`11.	      public void run() {`

`12.	        for(int x = 0; x < 10000; ++x)`

`13.	          counter.increment(); `
`14.	        `
`15.	       `
`16.	        `
`17.	        //`
`18.	      }`
`19.	    }`
`20.	    
`21.	    
`22.	    CountingThread t1 = new CountingThread();`

`23.	    CountingThread t2 = new CountingThread();`

`24.	    t1.start(); t2.start();`

`25.	    t1.join(); t2.join();`

`26.	    System.out.println(counter.getCount());`

`27.	  }`

`28.	}`
two threads accessing the same data and incrementing it, counter is supposed to total 20,000 but counter remained less then 20,000 throughout the run of the application.

## Assignment:
Reading Chapters
Butcher Ch. 1, Butcher Ch. 2, Butcher Ch. 3-4

# Week 2
## Threads and Locks

### Dining Philosopher’s Problem

	Philosopher thinks for a while, grab left chopstick, grabs right chopstick and eats for a while.
	Possible deadlock when all of them grab left chopstick at the same time and wait forever for the right one.
	Solution can be by dropping the left one after certain amount of time and synchronize both chopsticks, instead of first left then right.

	### WordCount
	For the case of this program, a Wikipedia dump has been downloaded of the size 34mb. file enwiki.xml

	### WordCountConsumer

	Reads 100000 pages and puts each page in the queue. Whenever the queue is full then it waits.
	Time running:  19348ms

	### WordCount Synchronized
	Time running: 28000ms

	### WordCount Concurrent
	Using multiple cores.

	1 counter: 19684ms

	2 counters: 15304ms

	3 counters: 14070ms

	4 counters: 14114ms

	5 counters: 13719ms

	6 counters: 13854ms

	7 counters: 13823ms

	### WordCount Batch
	Using local hashmap.

	1 counter: 18982ms

	2 counters: 13368ms

	3 counters: 12426ms

	4 counters: 12341ms

	5 counters: 14304ms


## Assignment:
Read Chapters
Butcher Ch. 1, Butcher Ch. 2, Butcher Ch. 3-4


# Week 3
## Threads and Locks/ intro to functional programming
````(def alphabet-length 26)````
````(def letters (mapv (comp str char (partial + 65)) (range alphabet-length)))````

````(defn random-string````
    ````[length]````
    ````(apply str (take length (repeatedly #(rand-nth letters)))))````

````(defn random-string-list````
    ````[list-length ssstring-length]````
    ````(doall (take list-length (repeatedly (partial random-string string-length)))))````

````(def names (random-string-list 20000 10000))````
````(time (dorun (pmap clojure.string/lower-case names)))````


Changing the amount of words of length 10k

20k	10k	1107ms

10k	10k	513ms

5k	10k	303ms

Changing the size of the 20k of words

20k	10k	1159ms

20k	5k	556ms

20k	1k	171ms


Setup Leiningen
## Assignment:
lab 4-5 from Clojure from the ground up https://aphyr.com/tags/clojure-from-the-ground-up
Labs

Read Chapters
Meier, Ch 1-3, Butcher Ch 5-7, Butcher Ch. 8

# Week 4
## Functional Programming / Concurrency and parallelism
# in class :
### Exercises done in class
````(def letters (mapv (comp str char (partial + 65))(range alpha-length)))````

````map f seq //maps a function to the sequence````

````(map inc [1 2 3 4 5])````

````(def ran-string [length] (apply str (take length (repeatedly #(rand-nth letters)))))````

````(take 5 (repeatedly #(rand-nth letters)))````

````(def rand-str-list [list-length str-length] (doall (take list-length (repeatedly (partial ran-string str-length)))))````

> parallelisation overhead [ time taken to get cores ready to perform  a task]


MapReduce
````(reduce + [1 2 3])````

## Assignments:
lab 7 from Clojure from the ground up https://aphyr.com/tags/clojure-from-the-ground-up 
install cloudera with virtualbox

Read Chapters
Butcher Ch. 8

# Week 5
no class
# Week 6
## Lambda Architecture
in class:

started with examples on the functional programming ( lab on  https://aphyr.com/posts/306-clojure-from-the-ground-up-state)

explained during the exercise : delay and promises, for promised with status pending, defer/@ on the promise can cause deadlock

### Chosen Topic: **OpenCL** (Data Parallelism ch. 7)
