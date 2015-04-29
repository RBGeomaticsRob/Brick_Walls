#Node#

Some notes on node as I learn new things about it ...

##Creating a server and reading a file##

```node
var http = require('http');
var fs = require('fs');

http.createServer(function(request, response) {
  response.writeHead(200);
  fs.readFile('index.html', function(error, contents){
    response.write(contents);
    response.end();
  });
}).listen(8080);
```

##Creating custom event emitters##

```node
var events = require('events');
var EventEmitter = events.EventEmitter;
var chat = new EventEmitter();

chat.on('message',function(message){
  console.log(message)
});

chat.emit('message', 'Custom Message') // calls the message event to trigger listener
```

##Using events to work with the Server##

```node
var http = require('http');

var server = http.createServer();
server.on('request', function(request, response) {
  response.writeHead(200);
  response.write("Hello, this is dog");
  response.end();
});
// note: you can have multiple listeners on the same event
server.on('request', function(request, response){
  console.log("New request coming in...");
});

server.on('close', function(){
  console.log("Closing down the server...");
});

server.listen(8080);
```

##Pipe and Streams##

Pipe chunks up the file to be sent across the stream meaning that the whole file is never held in the server, but goes from the client through the server to the storage.

This is the code for reading a file and logging it to the terminal (piping it to the terminal), the long way.

```node
var fs = require('fs');
var file = fs.createReadStream("donkey.txt");
file.on('readable',function(){
  var chunk = null;
  while(null !== (chunk = file.read())){
    console.log(chunk.toString());
  }
});
```
This can and should be simplified by using a pipe this example reads from one file and writes to a destination file.

```node
var fs = require('fs');

var file = fs.createReadStream('origin.txt');
var destFile = fs.createWriteStream('destination.txt');

file.pipe(destFile, {end: false});
```

A further example would be to use a pipe to send data to a response in a web server environment:

```node
var fs = require('fs');
var http = require('http');

http.createServer(function(request, response) {
  response.writeHead(200, {'Content-Type': 'text/html'});

  var file = fs.createReadStream('index.html');
  file.pipe(response);
}).listen(8080);
```

###Pipe and end###
Pipe automatically closes a stream after writing, this can be avoided if required and a custom listener on end implemented as follows

```node
file.pipe(destFile, {end: false});

file.on('end', function(){
  destFile.end('Finished!');
});
```
