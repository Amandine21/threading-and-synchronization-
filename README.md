# Threading and Snchronization-
&emsp; The purpose of this programming assignment is to rework the server-client programming
assignment client’s program to use threads instead of processes when it comes to processing
data from the server. The original approach took a significant amount of run time since the
processes were running in a certain order, however, threading is a much more efficient
approach in regards to time.\
&emsp; When it comes to implementing the threads the first thing was to implement the Bounded
Buffer class in order to make a thread-safe buffer. These buffers contained both a push and pop
function that was used to push thread objects. A mutex was used to protect the critical section in
each function, and two synchronization variables were used to notify other threads under certain
conditions. These buffers were used as the request buffer which housed the patient thread’s
data, and the response buffer which contained the calculated structure values from the worker
thread.\
&emsp; The patient thread contained a function that generated a given amount of data points
that was passed from the main under the variable flag “n” from the getopt which represents the
number of requests. The purpose of the worker thread was to process the data from popping
the request buffer and sending the data in a FIFO channel to the server to obtain data in the
form of a structure and push it to the response buffer. This was done in the worker_thread
function which took in the FIFO channel, Request buffer, Response buffer, and buffers length
arguments. The function was implemented in an if-else if condition under an infinite while loop
which would make this function multi-functional. The first if statement will compare if the
message is equivalent to DATA_MSG, and if this is true it would create a structure object from
the values calculated from the server and push it into the response buffer. The else if statement
will compare if the message is equivalent to the FILE_MSG, and if this is true it will open the file
in the received directory and start writing the structure objects to it using the fseek() function. In
the last else-if statement it will compare the message to QUIT_MSG, and it will delete the
channel along with breaking out of the infinite loop.\
&emsp; This histogram thread function took the request buffer and histogram collection named
histogram channel as the arguments.The function was implemented by using a for loop to pop
the response buffer that contained the structured pairs and update the histogram collection. The
file_transfer thread function was almost the exact same implementation from the first
programming assignment with little changes such as instead of using cread and cwrite it is
pushed to the request buffer.\
&emsp; Finally the main was implemented by first creating a file request signal, and initializing it
to false unless the “f” flag was used then it will be set to true. Then a vector will be created with
the FIFOrequestchannel pointer data type, and will have a size of the number of workers being
passed from the flag w. A for loop will be used to assign each element in the vector its own
FIFO channel to ensure each worker gets its own channel. Next in the timeval, a vector of
threads will be created with the size of the number of patients being passed from the flag p.
Then the file thread will be created. Next there will be an if-else statement that will check to see
if the file request signal is false. If this is true a for loop will be used to assign the elements in the
vector patients to launch their threads with the incrementer in the loop plus one representing the
patient number in the patient_thread function. If the condition does not hold then the file thread
will be assigned to the file_thread_function.\
&emsp; Finally, the threads need to be joined to ensure the program will not finish without them
and avoid race conditions as well. This will have to be done in a particular order, in order to
maintain the system. A for loop was used to iterate through the vector of worker threads, and
the join function was used to wait on the threads to complete. Next, if the file request signal was
false then the paired data struct with the value 0 for both member variables would be pushed to
the response buffer in for a loop. Finally, a for loop will be used to iterate through the histogram
vector and the join function will be used to ensure the threads are completed, Finally, the
program will be outside of the timeval which will get the histogram data and its time to calculate
the data from every thread. The amount of time will vary since the total number of threads is
p+w+h which could vary from the command line


<h2>Observation and Exlpanation:</h2>
&emsp; 1) If there are more worker threads this will increase performance which will reduce the
amount of time to work on a given task.

&emsp; 2) If there are more data points this will take much more time to process the information.

&emsp; 3) Fewer histograms will take more time however more histograms it result in a constant
time. The histograms are not as heavily time-dependent as the worker thread and file
transfer thread but can make a significant difference if there are few of them.


