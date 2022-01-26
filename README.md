# Pure ALOHA (without back off)

This code simulates a pure ALOHA system but without back off policy. Each thread is a sender. At first program asks you for frame transmition time and duration of simulation(both in seconds).
Program consists of one crutial function which acts like a sender in the system.
```python
def runALOHAnode():
  ...
  ...
```
There is no receiver reciever considered in thiis program and the focus is on senders and the transmission channel. we store the state of transmission channel in programm. At first channel is not busy because no one is sending.
```python
channelBusy = False
```
We know that senders in pure ALOHA wait for a random exponential value of time(in our case in miliseconds). To generate such random variables we used  ``` waitTime =  random.exponential(0.5,None)``` with lambda = 1/2.
After waiting, node starts transmitting(Whether channel is busy or not).
When a node is sending it acquires a lock and sets ```channelbusy``` to True. It needs Tfr seconds to send a frame completely. In this time if one or more nodes start sending then are the sending frames are assumed lost!
When a node starts sending while the lock is acquired(channel is busy) then we put a message(integer 1) in a queue which is shared between nodes.
```python
if lock.locked():
    #Collision happens but still transmiting the frame
    failedFrames += 1
    #Put data in queue to tell other sending nodes that this is a collision and their transmitted frame is failed
    q.put(1)
    #Sleeping for Tfr seconds means that node is transmitting and it takes Tfr seconds.
    sleep(Tfr)
```
The first node which acquired the lock relizes that other/s nodes are sending their frames too by seeing the none empty queue.
```python

if q.empty():
    pass
    #q being not empty means some other node(s) are transmitting too and this transmission is failed because of collision.
else:
    collision = True
```
At the end before killing all thrread using ```sys.exit()``` ,we calculate the throughput of system using:
```python
print("Throughput(kbps): " + str((successfulFrames)/duration))
```
