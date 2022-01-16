## Outline

* Theory of parallelism: work, span, Amdahl’s Law, weak vs. strong scaling, data races, determinism
* Task parallelism using Java’s ForkJoin framework
* Functional parallelism using Java’s Future interface
* Loop-level parallelism using Java 8 Streams
* Dataflow parallelism using data-driven tasks

## Resources:

* Java 8 Javadocs: https://docs.oracle.com/javase/8/docs/api/

* PCDP Javadocs: https://habanero-rice.github.io/PCDP/

* PCDP Source Code: https://github.com/habanero-rice/PCDP

* RecursiveAction: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/RecursiveAction.html

* RecursiveTask: http://docs.oracle.com/javase/8/docs/api/?java/util/concurrent/RecursiveTask.html

* ForkJoinPool: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html

* Java Streams Javadocs: https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html

* The Java Stream class: https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html

* A simple Java tutorial on streams: http://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/

* A simple Java ForkJoin tutorial: https://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html

* Using the JDK's performance profiler: http://docs.oracle.com/javase/7/docs/technotes/guides/visualvm/


### async and finish API
* `async` contains the API for executing a Java 8 lambda asynchronously. For example,

```java
async(() -> {S1; ...});
```
spawns a new child task to execute statements S1, ... asynchronously.

* finish contains the API for executing a Java 8 lambda in a finish scope. For example,

```java
finish(() -> {S1; ...});
```

executes statements S1, ..., but waits until all (transitively) spawned asyncs in the statements’ scope
have terminated.


###  Creating Future  Tasks  in Java’s  Fork/Join Framework

Some key differences between future tasks and regular tasks in the Java’s Fork/Join (FJ) framework are as follows:

1. A future task extends the `RecursiveTask` class in the FJ framework, instead of `RecursiveAction` as in regular tasks.

2. The `compute()` method of a future task must have a non-void return type, whereas it has a void return type  for  regular tasks.

3. A method call like `left.join()` waits for the task referred to by object `left` in both cases, but also provides the task’s return value  in the case of future tasks.

### Difference between RecursiveTask and RecursiveAction in ForkJoinPool
* `RecursiveTask` returns a value, `RecursiveAction` doesn't. It's like `Task` vs `Runnable`.
* Although technically, `RecursiveAction` does return a value, it's just always null, because it's a `ForkJoinTask<Void>`, and that's the only possible value of Void.
* REF: https://stackoverflow.com/questions/50817843/difference-between-recursive-task-and-recursive-action-in-forkjoinpool

Refer https://www.baeldung.com/java-fork-join for examples.

### Functional Parallelism - Java Streams

Lecture Summary: Let's see how Java streams provide  a  functional approach to operating on collections of data. For example, the statement, `students.stream().forEach(s -> System.out.println(s));`, is a succinct way of specifying an action to be performed on each element `s` in the collection, students.  An aggregate data query or data transformation can be specified by  building a stream  pipeline consisting of a source (typically by  invoking the `.stream()` method on a data collection, a sequence of intermediate operations such as `map()` and `filter()`, and an optional terminal operation such as `forEach()` or `average()`.  As an example, the following pipeline can be used to compute the average age of all active  students using Java streams:

```java
students.stream()
    .filter(s -> s.getStatus() == Student.ACTIVE)
    .mapToInt(a -> a.getAge())
    .average();
```

From the viewpoint of this course, an important benefit of using Java  streams when possible is that the  pipeline can be made to execute in parallel by designating the source to be a parallel stream, i.e., by simply replacing `students.stream()` in the above code by `students.parallelStream()` or `Stream.of(students).parallel()`. This form of functional parallelism is a major convenience for the programmer, since they do not need to worry about explicitly allocating intermediate collections (e.g., a collection of all active students), or about ensuring that parallel accesses to data collections are  properly synchronized.

#### Determinism and Data Races
A parallel program is said to be functionally deterministic if it always computes the same answer when given the same input, and structurally deterministic if it always computes the same computation graph, when given the same input. The presence of data races often leads to functional and/or structural nondeterminism because a parallel program with data races may exhibit different behaviors for the same input, depending on the relative scheduling and timing of memory accesses involved in a data race. In general, the absence of data races is not sufficient to guarantee determinism.
