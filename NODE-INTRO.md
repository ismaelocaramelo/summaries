## NODE - 1

> Allows you to build scalable network applications using Javascript on the server-side. It´s fast because it´s mostly C code.

### What could you build ?

* Websocket server
* Fast File Upload Client
* Ad Server
* Any Real-Time Data Apps

> Key points

* NodeJs uses Javascript which is single-threaded (one command at time) and allows us non-blocking code, nodejs become powerful.

### The Event Loop

The event loop is what allows Node.js to perform non-blocking I/O operations. When Node.js starts, it initializes the event loop, processes the provided input script. It has several phases which procedures in order. Each phase has a FIFO (First in First Out) queue of callbacks to execute.

* timers: this phase executes callbacks scheduled by setTimeout() and setInterval().
* I/O callbacks: executes almost all callbacks with the exception of close callbacks, the ones scheduled by timers, and setImmediate().
* idle, prepare: only used internally.
* poll: retrieve new I/O events; node will block here when appropriate.
* check: setImmediate() callbacks are invoked here.
* close callbacks: e.g. socket.on('close', ...).

### Events in Node

> Many objects in Node emit events

* All objects that emit events are instances of the EventEmitter class. These objects expose an eventEmitter.on() function that allows one or more functions to be attached to named events emitted by the object.

* When the EventEmitter object emits an event, all of the functions attached to that specific event are called synchronously. Any values returned by the called listeners are ignored and will be discarded. Althought functions can switch to an asynchronous mode of operation using the setImmediate() or process.nextTick()

### Streams

> They are like channels where the data flows. Streams can be readable, writeable, or both. All streams are instances of EventEmitter. They emit events that can be used to read and write data. However, we can consume streams data in a simpler way using the pipe method.

> Types of streams

* Readable - streams from which data can be read (for example fs.createReadStream()).
* Writable - streams to which data can be written (for example fs.createWriteStream()).
* Duplex - streams that are both Readable and Writable (for example net.Socket).
* Transform - Duplex streams that can modify or transform the data as it is written and read (for example zlib.createDeflate()).

### Buffering

* Both Writable and Readable streams will store data in an internal buffer that can be retrieved using writable.\_writableState.getBuffer() or readable.\_readableState.buffer, respectively.
* Every readable has to be consumed if not data will sit in the internal queue and if reaches the threshold will stop reading until is consumed. (readable.read())
* Every Writable stream when reached the threshold writable.write() will return false.

> Key points

* Streams are really powerful when working with large amounts of data, or data that’s coming from an external source one chunk at a time.
