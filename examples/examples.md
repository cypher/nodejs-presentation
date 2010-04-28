!SLIDE

# Examples
## (using Node.js 0.1.91)

!SLIDE code small

	@@@ javascript
	// Delayed hello world

	var sys = require("sys");

	setTimeout(function () {
	  sys.puts("world");
	}, 2000);

	sys.puts("hello");

	process.addListener("SIGINT", function () {
	  sys.puts("good bye");
	  process.exit(0)
	});

!SLIDE code smaller

	@@@ javascript
	// Load the sys module for console writing.
	var sys = require('sys');
	// Load the http module to create an http server.
	var http = require('http');

	// Configure our HTTP server to respond with Hello World
	// to all requests.
	var server = http.createServer(function (request, response) {
	  response.writeHead(200, {"Content-Type": "text/plain"});
	  response.write("Hello World\n");
	  response.end();
	});

	// Listen on port 8000, IP defaults to 127.0.0.1
	server.listen(8000);

	// Put a friendly message on the terminal
	sys.puts("Server running at http://127.0.0.1:8000/");

!SLIDE code smaller

	@@@ javascript
	var stat = require("fs").stat,
		puts = require("sys").puts;

	stat("stat.js", function (err, s) {
	  puts("modified: " + s.mtime);
	});

!SLIDE code smaller

	@@@ javascript
	// Streaming http
	var http = require("http");

	http.createServer(function (req,res) {
	  res.sendHeader(200, {"Content-Type": "text/plain"});
	  res.write("Hel");
	  res.write("lo\r\n");

	  setTimeout(function () {
		res.write("World\r\n");
		res.end();
	  }, 2000);
	}).listen(8000);

!SLIDE code smaller

	@@@ javascript
	var tcp = require("net");
	var server = tcp.createServer(function (socket) {
	  socket.setEncoding("utf8");
	  socket.addListener("connect", function () {
		socket.write("hello\r\n");
	  });
	  socket.addListener("data", function (data) {
		socket.write(data);
	  });
	  socket.addListener("end", function () {
		socket.write("goodbye\r\n");
		socket.end();
	  });
	});
	server.listen(7000, "localhost");

!SLIDE code smaller

	@@@ javascript
	var sys=require('sys'), http = require('http');
	var connection = http.createClient(80, "search.twitter.com");
	var since = 0, target = "@viennajs";
	function getTweets() {
	  var request = connection.request('GET', "/search.json?q=" +
						target + "&since_id="+since,
						{
							"host": "search.twitter.com",
							"User-Agent": "NodeJS HTTP Client"
						});
	  request.addListener("response", function(response) {
		var responseBody = "";
		response.setBodyEncoding("utf8");
		response.addListener("data", function(chunk) {
			responseBody += chunk;
		});
		response.addListener("end", function() {
		  tweets = JSON.parse(responseBody);
		  var results = tweets["results"],
		  length = results.length;
		  for (var i = (length-1); i >= 0; i--) {
			if (results[i].id > since) {
			  since = results[i].id;
			}
			sys.puts("From " + results[i].from_user +
					 ": " + results[i].text);
		  }
		});
	  });
	  request.end();
	  setTimeout(getTweets, 10000);
	};
	getTweets();

!SLIDE code smaller

	@@@ javascript
	// Non-blocking HTTP proxy
	var http = require('http');

	http.createServer(function(request, response) {
	  var proxy = http.createClient(80, request.headers['host'])
	  var proxy_request = proxy.request(request.method,
		                                request.url,
		                                request.headers);
	  proxy_request.addListener('response', function (proxy_response) {
	    proxy_response.addListener('data', function(chunk) {
	      response.write(chunk);
	    });
	    proxy_response.addListener('end', function() {
	      response.end();
	    });
	    response.writeHead(proxy_response.statusCode,
		                   proxy_response.headers);
	  });
	  request.addListener('data', function(chunk) {
	    proxy_request.write(chunk);
	  });
	  request.addListener('end', function() {
	    proxy_request.end();
	  });
	}).listen(8080);

Source: [A HTTP Proxy Server in 20 Lines of node.js Code](http://catonmat.net/http-proxy-in-nodejs)
