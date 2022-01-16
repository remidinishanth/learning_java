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


###  Creating Future  Tasks  in Java’s  Fork/Join Framework

Some key differences between future tasks and regular tasks in the Java’s Fork/Join (FJ) framework are as follows:

1. A future task extends the `RecursiveTask` class in the FJ framework, instead of `RecursiveAction` as in regular tasks.

2. The `compute()` method of a future task must have a non-void return type, whereas it has a void return type  for  regular tasks.

3. A method call like `left.join()` waits for the task referred to by object `left` in both cases, but also provides the task’s return value  in the case of future tasks.

### Difference between RecursiveTask and RecursiveAction in ForkJoinPool
* `RecursiveTask` returns a value, `RecursiveAction` doesn't. It's like `Task` vs `Runnable`.
* Although technically, `RecursiveAction` does return a value, it's just always null, because it's a `ForkJoinTask<Void>`, and that's the only possible value of Void.
* REF: https://stackoverflow.com/questions/50817843/difference-between-recursive-task-and-recursive-action-in-forkjoinpool
