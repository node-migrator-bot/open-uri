# Asynchronous Open URI

  A CommonJS module inspired by Rubys Open-URI library.


## Install

  It's available on npm, so a simple `npm install open-uri` should be enough.


## Usage

	var open = require("open-uri");

	// Get a website
	open("http://google.com",function(err,google){console.log(google)})

	// Get a file
	open("/var/log/system.log",function(err,log){console.log(log)})

	// Stream a file to STDOUT
	open("file:///var/log/system.log",process.stdout)

	// Stream a website to a file
	var file = require("fs").createWriteStream("/tmp/goog.html")
	open("https://encrypted.google.com/search?q=open+uri",file)

	// Chain it 
	open("http://google.com",console.log)("http://publicclass.se",process.stdout)

	// Get a file off of an FTP
	open("ftp://user:pass@ftp.example.com/myfile.txt",function(err,txt){console.log(txt)})


## Supported schemes

  http, https, file & ftp

Uses the [addressable module](https://github.com/publicclass/addressable) for parsing the scheme and the [mime module](https://github.com/bentomas/node-mime) for parsing the data.


### Options for all schemes

* `stream`    (boolean) For when you only want the callback to receive a stream instead of the complete body as the second argument. Defaults to `false`.


### Options for HTTP(S)

* `follow`    (boolean) Follow redirects. Defaults to `true`.
* `gzip`      (boolean) If the request should be attempted with gzip. Defaults to `true` if the [node-compress module](https://github.com/waveto/node-compress) is available.
* `headers`   (object) Headers to pass along with the HTTP request.


### Options for file

* `encoding`  (string) The encoding of the file to read.


### Options for FTP

* `anonymous` (boolean) If no user info is in the URI, add anonymous. Defaults to `true`.



## History


### 0.2.2

* [Feature] Re-factored the schemes into separate files. Now adding more schemes will be much easier.

* [Feature] Added a simple binary. Usage: `open-uri http://google.com`

* [Fix] _FTP_ is now closing properly when completed or failed.

### 0.2.1

* [Feature] Better parsing of content using the [mime module](https://github.com/bentomas/node-mime) for Content-Type lookup.

* [Feature] _FTP_ Initial support using the [node-ftp module](https://github.com/mscdex/node-ftp).


### 0.2.0

* [Feature] _HTTP(S)_ Now supports gzip responses (and adds a 'Accept-Encoding: gzip'-header if there is none already).

* [Feature] _HTTP(S)_ Now decodes JSON if response is of 'Content-Type: application/json'.

* [Fix] _HTTP(S)_ Fixed an issue with redirects.


### 0.1.0

* Initial version.


## TODO

*  _HTTP(S)_ Support older versions of Node? Currently only support 0.3.6 and up (because of the new HTTP Client API).

*  _HTTP(S)_ Proxy support?

* _HTTPS_ Requesting HTTPS required certificate and private key, how should these be provided? Should look for them in:
    1. In the options as a normal https.get(). Should it expand them if it's a path? I.e. {key: "./key.pem", cert: "./cert.pem"} is fs.readSync()-ed?
    2. ENV-variables to their paths, NODE_HTTPS_KEY=./key.pem NODE_HTTPS_CERT=./cert.pem
    3. Check for them in the process pwd.

* A timeout option for all schemes would be useful.

* A binary, just for fun really. Usage: `open-uri http://google.com`. 
  - Output to process.stdout by default. 
  - Allow a REST interface? So if something is written to stdin it's sent as body to the url using POST by default, but definable with -X (curl style!)
  - Pass command line arguments as options to the scheme functions. Ex. `open-uri -gzip http://google.com` sets opts.gzip to true.

* More schemes support? Suggestions?
  - SQL query. Is there a standard URI for this? Something like: `mysql://root@localhost/mydb?query=SELECT * FROM x;` or `sqlite3://file.sqlite?query=SELECT * FROM x;` possibly with support for input escaping. open("sqlite3://file.sqlite?query=SELECT * FROM x WHERE id=? AND name=?;",[12,"bob"]) using the drivers own escaping. With a callback like `function(err,rows,meta){}`.
  - NNTP (using [node-nntp module](https://github.com/mscdex/node-nntp))


## Thanks to

* [Mikeal Rogers](https://github.com/mikeal), for his excellent [request module](https://github.com/mikeal/request/).

* [TJ Holowaychuck](https://github.com/visionmedia), for his minimalist projects inspiring this little tool.

* [Brian White](https://github.com/mscdex), for [ftp](https://github.com/mscdex/node-ftp) and [nntp](https://github.com/mscdex/node-nntp) modules.


## License 

(The MIT License)

Copyright (c) 2011 Robert Sk&ouml;ld &lt;robert@publicclass.se&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.