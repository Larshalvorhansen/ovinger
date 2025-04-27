NOTE TO SELF:
Gjor denne ferdig en annen dag. Det som gjenstår er det som står nedenfor her. 
Det som allerede er gjort er det som står i den orginale README men som er fjernet her.



Exercise 1 : Concurrency Essentials
===================================

4: Sharing a variable, but properly
-----------------------------------

Modify the code from the previous part such that the final result is always zero.

In your solution, make sure that the two threads intermingle, and don't just run one after the other. Running them sequentially would somewhat defeat the purpose of using multiple threads (at least for real-world applications more interesting than this toy example).

*It may be useful to change the number of iterations in one of the threads, such that the expected final result is not zero (say, -1). This way it is easier to see that your solution actually works, and isn't just printing the initial value after doing nothing.*

### C

 - POSIX has both mutexes ([`pthread_mutex_t`](http://pubs.opengroup.org/onlinepubs/7990989775/xsh/pthread.h.html)) and semaphores ([`sem_t`](http://pubs.opengroup.org/onlinepubs/7990989775/xsh/semaphore.h.html)). Which one should you use? Add a comment (anywhere in the C file, or in the `results.md` file) explaining why your choice is the correct one.
 - Acquire the lock, do your work in the critical section, and release the lock.
 - Reminder: Make sure that the threads get a chance to interleave their execution.


### Go

Using shared variable synchronization is possible, but not the idiomatic approach in Go. You should instead create a "server" that is responsible for its own data, [`select{}`](http://golang.org/ref/spec#Select_statements)s messages, and perform different actions on its data when it receives a corresponding message. 

In this case, the data is the integer `i`, and the three actions it can perform are increment, decrement, and read (or "get"). Two other goroutines should send the increment and decrement requests to the number-server, and `main` should read out the final value after these two goroutines are done.

Before attempting to do the exercise, it is recommended to have a look at the following chapters of the interactive go tutorial:
 - [Goroutines](https://tour.golang.org/concurrency/1)
 - [Channels](https://tour.golang.org/concurrency/2)
 - [Select](https://tour.golang.org/concurrency/5)

Remember from before where we had no good way of waiting for a goroutine to finish? Try sending a "finished"/"worker done" message from the workers back to main on a separate channel. If you use different channels for the two threads, you will have to use `select { /*case...*/ }` so that it doesn't matter what order they arrive in, but it is probably easier to have multiple senders on the same channel that is read twice by `main`. 

*Hint: you can "receive and discard" data from a channel by just doing `<-channelName`.*

---

Commit and push your code changes to GitHub.

5: Bounded buffer
-----------------

From the previous part, it may appear that message passing requires a lot more code to do the same work - so naturally, in this part the opposite will be the case. In the folder [bounded buffer](./5%20-%20bounded%20buffer) you will find the starting point for a *bounded buffer* problem.

The bounded buffer should work as follows:
 - The `push` functionality should put one data item into the buffer - unless it is full, in which case it should block (think "pause" or "wait") until room becomes available.
 - The `pop` functionality should return one data item, and block until one becomes available if necessary.

### C

The actual buffer part is already provided (as a ring buffer, see `ringbuf.c` if you are interested, but you do not have to edit - or even look at - this file), and your task is to use semaphores and mutexes to complete the synchronization required to make this work with multiple threads. If you run it as-is, it should crash when the consumer tries to read from an empty buffer.

*If you are working from home and need a C compiler online, [try this link](https://repl.it/@klasbo/ScientificArcticInstance#main.c). It should be instanced with the full starter code.*

The expected behavior (dependent on timing from the sleeps, so it may not be completely consistent):
```
[producer]: pushing 0
[producer]: pushing 1
[producer]: pushing 2
[producer]: pushing 3
[producer]: pushing 4
[producer]: pushing 5
[consumer]: 0
[consumer]: 1
[producer]: pushing 6
[consumer]: 2
[consumer]: 3
[producer]: pushing 7
[consumer]: 4
[consumer]: 5
[producer]: pushing 8
[consumer]: 6
[consumer]: 7
[producer]: pushing 9
   -- program terminates here(-ish) --
```


### Go

Read [the documentation for `make`](https://golang.org/pkg/builtin/#make) carefully. Hint: making a bounded buffer is one line of code. 

Modify the starter code: Make a bounded buffer that can hold 5 elements, and use it in the producer and consumer.

The program will deadlock at the end (main is waiting forever - as it should, and the consumer is waiting for a channel no one is sending on). Since this is a toy example, don't worry about it. But if you have any plans on doing more work with the Go language, you should take a look at the error message and try to understand it, as it will help you debug any such problems in the future.

---

As usual - commit and push your changes to GitHub.

6: Some questions
-----------------

The file [*questions*](/questions.md) contains a few questions regarding some of the concepts this exercise covers, as well as some broader engineering questions. Modify the file with your answers.

You do not need "perfect" or even complete answers to these questions. Feel free to ask the student assistants (even during the exercise approval process) to discuss any questions you get stuck on - you might find you learn more in less time this way.

