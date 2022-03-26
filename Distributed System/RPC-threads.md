# RPC and Threads

## Why Go?
This course will use Go as the main programming language. The features of Go:
- Good support for threads
- Convenient RPC
- Type safe
- Garbage-collected (no use after freeing problems)
- Threads + GC is particularly attractive!
- Relatively simple
- After the tutorial, use https://golang.org/doc/effective_go.html

<br>

## Thread
```Thread = "thread of execution"```. 
- They allow one program to **do many things** at once
- Each thread executes **serially**, just like an ordinary non-threaded program
- The threads **share memory**
- Each thread includes some **per-thread state**: ```Program counter, Registers, Stack```


### Why Thread?
<table>
    <tr> 
        <th> Reason </th>  
        <th> Description </th>
    </tr>
    <tr> 
    	<td> Express Concurrency </td>   
		<td> The ability of an algorithm or program to run more than one task at a time </td>
	</tr>
	<tr> 
		<td> I/O concurrency </td>   
		<td> Client sends requests to many servers <b>in parallel</b> and waits for replies <br>
             Server processes multiple client requests <b>in parallel</b>; each request may block <br>
             While waiting for the disk to read data for client X, process a request from client Y
        </td>
    </tr>
	<tr>
        <td> Multicore Performance </td>   
        <td> Execute code in parallel on several cores </td>
    </tr>
    <tr>
        <td> Convenience </td>   
        <td> In background, once per second, check whether each worker is still alive </td>
    </tr>
</table>


### Alternative to Threads
- Write code that explicitly interleaves activities, in a single thread. Usually called ```"event-driven."```
- Keep a table of state about each activity, e.g. each client request. <br>
  One "event" loop that:
  	- checks for new input for each activity (e.g. arrival of reply from server),
    - does the next step for each activity,
    - updates state.
- Event-driven gets you I/O concurrency, 
	and eliminates thread costs (which can be substantial),
	but doesn't get multi-core speedup,
    and is painful to program.
	
	
### Threading Challenges
<table>
    <tr> 
        <th> Challenges </th>  
        <th> Description </th>
    </tr>
	<tr> 
		<td> Shared Data  </td>   
		<td> E.g. What if two threads do n = n + 1 at the same time? <br>
			 Or one thread reads while another increments? <br>
			 This is a "race" -- and is usually a bug. <br>
			 <b> Solution: use locks (Go's sync.Mutex) or avoid sharing mutable data. </b>
		</td>
    </tr>
    <tr> 
    	<td> Coordination Between Threads  </td>   
		<td> E.g. One thread is producing data, another thread is consuming it. <br>
			 How can the consumer wait (and release the CPU)? <br>
			 How can the producer wake up the consumer? 
	  	</td>
	</tr>
	<tr> 
		<td> Deadlock </td>   
		<td> Cycles via locks and/or communication (e.g. RPC or Go channels) </td>
    </tr>
</table>
