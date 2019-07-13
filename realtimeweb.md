# Realtime Web

In the current world of the internet, we want data fast. We want it now! So most modern applications ask for a realtime solution. But how do we achieve this? 

## Polling/Long polling

Polling is an older technology for creating realtime applications. It's a technique where we ask the server for data. But even polling can be divided between polling and longpolling.

### Polling 

Polling is where we continuously ask the server for new data. Examplecode below.

```javascript

var poll = function() {
	var xhr = new XMLHttpRequest();

	xhr.load = function() {
		//here we do something with the response
	}

	xhr.open("get","http://letspollthisurl.test", true);
	xhr.send();
}

setInterval(function() {
	poll();
},20000)

```

The code above will poll the url every 20 seconds for new data. This is OK if you want to look for data on a server that you do not have control over. But the problem here is, that it's possible to miss data. Updates can occur between those 20 seconds. So it's possible to miss data this way. And it's also very heavy on the machine since your computer has to continuously make new requests.

### Longpolling

longpolling is a better solution than longpolling because we can update the user in a single request before the server timesout the request. But you have to have control over the server. Let's see the example below.

Server code:

```javascript

app.get("/longpoll", function(req,res){
	//we are using a timeout here to demonstrate that the server has control over the request. The server will respond 20 seconds after the request. But you can basicly create any implementation. For example on user input.
	setTimeout(function(){
		res.send("here is new freshly updated data");
	},20000)
});

```

Client code:

```javascript
var longPoll = function() {
	var xhr = new XMLHttpRequest();

	xhr.load = function() {
		//in this code we handle the response but also directy create a new request. 
		longPoll();
	}

	xhr.abort = function() {
		//this code is for when the request has been timed out because there was no data available for too long. So the server wont store too many requests in memory.
		longPoll()
	}

	xhr.open();
	xhr.send()
}

```

The example above wont poll the server after X amount of time. But rather only when the server has new data available. This is more efficient than having to ask the server for data every X seconds. 

[You can see my github repo for a implementation of longpolling](https://github.com/Zekkie/browser-technologies-1819/tree/master/notifications)

## Websockets, realtime done right

With polling you can create a realtime application. But today we have Websockets. But what is a websocket? 

### A non closing connection

A websocket is simply said a connection that will never terminated unless done by the user or server. The connection gets a status code of 101, which means it's a pending connection(open). 
![WS Connection](https://i.gyazo.com/2d61272dc628b5d131c5001939ecee3c.png)

But why do we care about this? Well we can continuously retreive and send data without ever having to create e new request to the server. This both reduces server and client load and it's fully realtime, because the server can notify the client as soon as data is available. 

### Socket.io

[Socket.io](https://socket.io/) is a very nice websocket framework. It's confusing when you startout with it, because the code for the client is practically the same as for the server. Socket.io's code is pretty simple for sending and retreving data. it only as a on and emit method. We listen on "on" to retreive data and "emit" is used to send data.

```javascript

io.on("some event", function(data) => {
	// here we handle the incomming data
})

io.emit("some event", {data: "new data!"})

```
As you can see, it's pretty simple but that is how we get data and send data!

## Conclusion

To create a realtime application you have a few options. Personally I think Websockets are amazing, not only for simplicity but because it is truely realtime. But let's not forget about older technologies, because they will save you when Websockets are not an option. Or just for fun. 