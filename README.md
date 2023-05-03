Download Link: https://assignmentchef.com/product/solved-cs361-homework6-concurrent-programming
<br>
First of all, check out the skeleton code here: <a href="https://www.google.com/url?q=https%3A%2F%2Fclassroom.github.com%2Fa%2FPXO5kPCo&amp;sa=D&amp;sntz=1&amp;usg=AFQjCNFIYWelG8DAe2u9onyW08xwLv3MLg"><strong>htt</strong></a><a href="https://www.google.com/url?q=https%3A%2F%2Fclassroom.github.com%2Fa%2FPXO5kPCo&amp;sa=D&amp;sntz=1&amp;usg=AFQjCNFIYWelG8DAe2u9onyW08xwLv3MLg"><strong>p</strong></a><a href="https://www.google.com/url?q=https%3A%2F%2Fclassroom.github.com%2Fa%2FPXO5kPCo&amp;sa=D&amp;sntz=1&amp;usg=AFQjCNFIYWelG8DAe2u9onyW08xwLv3MLg"><strong>s://classroom.</strong></a><a href="https://www.google.com/url?q=https%3A%2F%2Fclassroom.github.com%2Fa%2FPXO5kPCo&amp;sa=D&amp;sntz=1&amp;usg=AFQjCNFIYWelG8DAe2u9onyW08xwLv3MLg"><strong>g</strong></a><a href="https://www.google.com/url?q=https%3A%2F%2Fclassroom.github.com%2Fa%2FPXO5kPCo&amp;sa=D&amp;sntz=1&amp;usg=AFQjCNFIYWelG8DAe2u9onyW08xwLv3MLg"><strong>ithub.com/a/PXO5kPCo</strong></a><a href="https://www.google.com/url?q=https%3A%2F%2Fclassroom.github.com%2Fa%2FPXO5kPCo&amp;sa=D&amp;sntz=1&amp;usg=AFQjCNFIYWelG8DAe2u9onyW08xwLv3MLg">.</a>

In this homework you will build a <strong>concurrent</strong> Airline Reservation System (ARS). The ARS provides essential functionalities:

Book: reserve a seat on a flight and issue a ticket

Cancel: cancel a ticket

Change: cancel a ticket and book another flight <strong>atomically</strong>

The ARS needs to work correctly in a concurrent environment where multiple threads can perform any of the above operations without external synchronization. Apparently, the ARS needs to take care of everything internally, such as acquiring locks.

A summary of your programming tasks:

Complete the implementation of ars.c to make it work in a single-threaded environment

(pass “./test” and “./main” without arguments)

Use a mutex lock to enforce thread safety and allow multiple threads to access the ARS simultaneously (pass “./main &lt;n&gt;”, where n &gt; 1)

Improve the scalability with fine-grained locks to allow concurrent processing on different flights (make “./main &lt;n&gt;” run much faster)

Enable wait-if-no-seat (book_flight_can_wait). User can wait until a seat is made available on the target flight (pass “./cond &lt;n&gt;”, where n &gt; 5)

https://sites.google.com/uic.edu/cs361summer2020/homeworks/hw6?authuser=1<strong>The skeleton code</strong>

<h1>The skeleton code</h1>

<a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">CS 361 Summer 2020</a>                            <a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">Home</a>         <a href="https://sites.google.com/uic.edu/cs361summer2020/schedule?authuser=1">Schedule</a>         <a href="https://sites.google.com/uic.edu/cs361summer2020/homeworks?authuser=1">Homeworks</a>

Re<a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">ad the functions in </a><a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">ars.c</a><a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">. In this s</a>ystem, a ticket is uniquely identified by a combination of <strong>&lt;user-id, flight-id, ticket-number&gt;</strong>. To obtain a ticket, a user (more accurately, an agent thread on behalf of the user) calls book_flight() with a user-id and a flight number. Once booked, the ARS will save the combination of the three numbers in an array associated to the flight data structure (flights[i]-&gt;tickets[j]). To cancel or change a ticket, user must provide the correct three-number combination. Accordingly, tickets are “nontransferable” because user-id must be the same.

ARS doesn’t maintain seat numbers. It only needs to make sure the flights are never overbooked.

The skeleton code contains most of the required data structures and functions. In your submission you can only add/change code in ars.c but not any other files. Similar to homeworks 4 and 5, the autograder will only copy and use your <strong>ars.c</strong> and ignore the other files in your submission. Feel free to change any files for debugging but remember to test with the original ones for your final solution.

test.c, main.c, and wait.c are programs that test the correctness and efficiency of the ARS. Your will need to make all of them work as expected.

<h1>Detailed tasks</h1>

<ol>

 <li>Your first task is to implement the change_flight function to make the system fully functional with one thread. To verify it works, compile the “<strong>test</strong>” program and run it. It will show “no error” if it works as expected. However, it is not thread-safe and if you compile and run “main” with an argument &gt;= 2, such as “make main; ./main 2” it will almost certainly fail due to race conditions (Murphy’s Law).</li>

 <li>A global mutex lock (biglock) is provided in ars.c but not used. Your second task is to employ this lock to provide thread-safety to the ARS. You need to perform lock/unlock in book/cancel/change functions. You <strong>don’t</strong> need to use them in ars_init() and dump_tickets(). Once this is done, “./main &lt;n&gt;” should work correctly with multiple threads (try ./main 2 and ./main 3).</li>

 <li>Have you observed the slowdown as the number of threads increases? Now, let’s look at the code in main.c. Each worker thread performs 10 million operations by random. Since ars uses one big lock, all of the incoming requests must execute sequentially. Can we make it more efficient by exploiting more concurrency? Of course. Since there is no shared data between different flights, we can give each flight its own lock which allows operations on different flights to run concurrently without blocking each other. Your third task is to replace the biglock with some per-flight mutex locks. Be careful with <strong>deadlocks</strong>! task is to replace the biglock with some per flight mutex locks. Be careful with <strong>deadlocks</strong>!</li>

</ol>

<a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">Once this is done, “./main 10” sha</a>ll finish with<a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">in 5 sec</a>o<a href="https://sites.google.com/uic.edu/cs361summer2020/schedule?authuser=1">nds.</a>

<a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">CS 361 Summer 2020</a>                            <a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">Home</a>         <a href="https://sites.google.com/uic.edu/cs361summer2020/schedule?authuser=1">Schedule</a>         <a href="https://sites.google.com/uic.edu/cs361summer2020/homeworks?authuser=1">Homeworks</a>

<ol start="4">

 <li><a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">The </a><a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">book_flight</a><a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1"> function will re</a>turn -1 if the flight is fully booked, which can be quite annoying. To better serve the customers, you need to roll out a new service called “book_flight_can_wait”, which always returns a valid ticket to the caller! If a flight is fully booked, the function must wait until some one have cancelled a ticket. You need to use <strong>condition variables</strong> to achieve this. You will need to replace the code in book_flight_can_wait() and add necessary structures/code to a few places in ars.c. Once this is done, “./wait &lt;n&gt;” should work correctly with any n &gt; 5. For example, “./wait 8” should finish within 5 seconds. See the lists below for available pthread library functions.</li>

</ol>

pthread functions that you can use:

pthread_mutex_init, pthread_mutex_lock, pthread_mutex_unlock pthread_cond_init, pthread_cond_wait, pthread_cond_signal

<strong>Don’t use the following functions:</strong>

pthread_cond_timedwait pthread_cond_broadcast

<strong>Debugging tips (in gdb):</strong>

While segmentation faults or failed assertions often print out a message to give you some clue, deadlocks can make you wait indefinitely without seeing anything. Sometimes you will have to use a debugger to hunt the bugs, which is often painful. Here are a few tips that might be useful if you’re using gdb from command line.

If program hangs there due to deadlocks, use “CTRL-c” to interrupt the program and inspect the execution context.

“tui enable”: a smart way to show source code.

“info threads” or “i th”: see all threads.

“thread 2” or “t 2”: select a thread in the list.

“bt”: see the stack backtrace of the current thread.

<span style="text-decoration: line-through;">“</span>frame 1″ or “fr 1”: switch to a function frame in the stack trace. main() should correspond to the largest frame number.

p                          g

<a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">“info locals” or “i lo”: inspect local </a><a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">CS 361 Summer 2020</a>        variables.              <a href="https://sites.google.com/uic.edu/cs361summer2020/home?authuser=1">Home</a>                                <a href="https://sites.google.com/uic.edu/cs361summer2020/schedule?authuser=1">Schedule</a>                           <a href="https://sites.google.com/uic.edu/cs361summer2020/homeworks?authuser=1">Homeworks</a>