---
layout: post
title:  "How to get a stack trace of a background thread in .Net"
date:   2022-02-04
categories: software development stacktrace c#
---
This blog entry is a response to a question I got in a stack overflow post: [How to get non-current thread's stacktrace?](https://stackoverflow.com/questions/285031/how-to-get-non-current-threads-stacktrace) \[In C# / .Net\] Specifically, I was asked if there were alternatives to the originally-obsolete solution: [https://stackoverflow.com/questions/285031/how-to-get-non-current-threads-stacktrace/9595704?noredirect=1#comment125211968\_9595704](https://stackoverflow.com/questions/285031/how-to-get-non-current-threads-stacktrace/9595704?noredirect=1#comment125211968_9595704).

The author of the solution, who also writes the renowned [LINQPad](https://www.linqpad.net/), recently revised his answer. I'd use care with following Joe's suggestion in shipping code, because what works correctly in a tool like LINQPad may not be appropriate for the environment that your application runs in. Specifically, Joe's suggestion may not work in the various sandboxes, non-admin, ect, environments that .Net applications frequently run in.  

A bit about my background. For almost a decade I was the desktop client lead for Syncplicity, a file synchronization product similar to Dropbox, OneDrive, and Google drive. While I led the desktop client, we were the most performant and scalable product on the marketplace. The desktop client was a highly multi-threaded C# desktop application. I have quite a lot of experience developing and debugging multithreaded .Net applications.  

I'm assuming the person who asked me for alternatives to getting another thread’s stack trace wants to log collect debugging information; or otherwise needs to fix some threading issues.  

## Is the Stack Trace for Logging?

I wish in .Net that it was very easy to get another thread's stack trace, because then it would be significantly easier to diagnose certain kinds of bugs like deadlocks and other situations where a thread is blocked. Then it would be easier to include defensive “bug detector” code to log what a thread is doing if it holds a lock for too long! Depending on your application's environment, you can't always create a process to obtain a stack trace.  

Unfortunately, the quick answer to “are there alternatives”, is that a multithreaded application needs a good logging strategy. The logger should always include the thread name, and thread ID. If you use async code, include Task.CurrentTaskId. Multi-server applications will also need to include the process ID, and the server instance number. Log4net can be configured to include these. Do not configure the logger to include stack traces, except in controlled debugging environments, because logging stack traces in every logging statement incurs extreme CPU overhead and performance will suffer.  

When examining logs, filter based on the thread ID, or Task.CurrentTaskId for async code. (And filter on machine ID and process ID for distributed code). This will provide a sequential list of each logging event from the thread. It may be necessary to add logging statements in troublesome areas of code to give yourself hints about what to search for in your logs.  

It may be _occasionally_ useful to include a stack trace (Environment.StackTrace) in carefully selected logging statements. Consider restricting logging stack traces to either debug builds or via some kind of a configuration setting. (Remember, logging a stack trace incurs a high performance penalty.)

**Important**: Any kind of logging, especially stack trace logging, will change timing of race conditions and deadlocks. Assuming that your bug is some kind of a threading issue, the logging you add could trigger a “Fermi's paradox” situation where you either reproduce the bug more, or less frequently, based on how you log!  

If you are directly creating threads, it's very useful to name your threads, and include the thread name in your logs. This makes it very easy to track down the specific thread in your log.  

If you are able to reproduce your problem when running your code inside of the Visual Studio debugger, when you break into the debugger, you can see a list of all your threads (and tasks). This also allows you to see each thread's stack trace! (Yay!) Assuming that you're logging the thread ID (and/or task Id), it should be very easy to find the specific thread that you are trying to debug.  

## Some Other Helpful multithreaded Techniques

If you are having problems with deadlocks or general threading issues, here are some additional strategies:  

* If you have a critical method that has a lot of callers, and could block for too long, add a string argument to the method. Give each call to the method a unique name. In your method, set a timer to log the string if the call takes longer than what you consider reasonable.
* Encapsulate your shared state so that it is prohibitively difficult to access it outside of a lock. For example, your shared state could be encapsulated in an object that is only accessible through a lambda. Then the object should have protections that prevent calling it from a different thread or holding on to the object outside of the lambda.
* Take the time to learn about programming with immutable values. If you have shared state, but the object itself is immutable, then it's safe to read from any thread without a lock. (Your code still needs to handle stale state.)
* If you are having problems with deadlocks, consider replacing the lock keyword with a pattern that includes timeouts. These exceptions can help you diagnose deadlocks, or at a minimum, allow you to gracefully recover from such a situation. For example: [https://github.com/GWBasic/ObjectCloud/blob/master/Server/ObjectCloud.Common/Threading/TimedLock.cs](https://github.com/GWBasic/ObjectCloud/blob/master/Server/ObjectCloud.Common/Threading/TimedLock.cs)

## Don’t Make Novice Mistakes

Now if you are trying to get a thread stack trace because you need to communicate among threads or wait for a thread to get into a particular state…Well… That's a novice mistake! (And depending on what you're doing, it might be time to admit you've gotten over your head!) Locking, monitors, wait handles, task completion sources, queues, fibers, mutexes, semaphores, critical sections, and other ways of coordination among threads are advanced programming techniques. Although I encourage you to learn, I really do recommend that you have a couple of years experience under your belt, or be in a more advanced computer science class, before you do multi-threaded programming in a professional setting.  

## Ok, but, really, is there a way to get another thread’s stack trace? (Without creating another process)

But if you really need to get another thread's stack trace at runtime when you don't have a debugger available, this is prohibitively difficult. I will try to explain why to the best of my ability.

I'm going to assume that you know a decent amount of multi-threaded programming, and that sharing state, specifically data structures, among running threads is very difficult. One thread just can’t safely read another thread’s data structures unless they are designed to be thread-safe; or locking is used. Often, allowing one thread to read another thread’s state incurs a performance penalty. An experienced software engineer can weigh the performance trade-offs of different approaches, such as locking versus lock free data structures. It's my professional opinion that designing .Net’s stack so that a stack trace can be read in a non-blocking manner will cause a prohibitive performance degradation to the entire .Net framework.  

This is why a thread needs to be suspended in order to get its stack trace from another thread. Why calling something like Thread.Suspend is problematic is more difficult for me to explain, because I have never worked with the internals of .net and the operating system. This response probably explains it best: [https://stackoverflow.com/a/3602878/1711103](https://stackoverflow.com/a/3602878/1711103).  

I do suspect that a very talented programmer could figure out how to modify .Net to allow getting other threads' stack traces at runtime without relying on an additional process. (After all, the garbage collector does walk the stack!) It would be very interesting to hear from one of Microsoft’s developers to know if this is feasible, and what kind of tradeoffs this would incur.  

## Conclusion

So there you have it: Use a logging strategy that includes information about each thread. Try to reproduce the bug inside a debugger, because the debugger will give you the stack trace of all threads. Make sure you have the experience to implement multi-threaded programming correctly. (Or be willing to admit that you've gotten over your head.) Given that .Net’s garbage collector walks threads’ stacks, I do wish that .Net allowed getting another thread’s stack trace for diagnostic purposes.
