# json-caching-proxy

NodeJs Proxy server written in Express that persists JSON responses to the file system and proxies everything else (e.g. .png, .html, .js, etc)

## Why this might be useful for developers?

* Mocking a new API - You can drop JSON files into the directory and the Proxy server will just read from the file as if the API existed. 
* Augmenting your development workflow with cached/mocked responses - Modifying UI code with underlying API/JSON Data from a remote server might staying the same.

## Installation

Command line: ``` $ npm install -g json-caching-proxy ```

Programatically: ``` $ npm install -D json-caching-proxy ```

## Command line usage

```js

json-caching-proxy [options]

  Options:

    -h, --help        output usage information
    -V, --version     output the version number
    -u, --url <url>   Remote server to proxy (e.g. https://network:8080)
    -p, --port [n]    Port for the local proxy server (Default: 3001)
    -d, --dir [path]  Local Directory to store JSON responses (Default: "cache")

```


### Example (command line)

```js

json-caching-proxy -u restful-api-server:8080 -d cache -p 8000

```

#### Controlling the Proxy once it's running

* http://localhost:8000/proxy/playback?enabled=`[true|false]` - Start/Stop replaying persisted JSON from the cache.
* http://localhost:8000/proxy/record?enabled=`[true|false]` - Start/Stop recording JSON to file system.
* http://localhost:8000/proxy/overwrite?enabled=`[true|false]` - Will overwrite existing cache that has been saved to the file system.
* http://localhost:8000/proxy/playback?clear` - Clears the file system cache. JSON will be deleted from the file system.


## Programmatic Usage

```js

const jsonCachingProxy = require('./');

let remoteServerUrl = 'api-server:8000';
let proxyPort = 3000;
let cacheDataDirectory = 'cache';
let outputLog = true;

let proxy = jsonCachingProxy({remoteServerUrl, proxyPort, cacheDataDirectory, outputLog});

proxy.start();

// Do stuff..

proxy.stop();

```

#### Using Express Middleware

```js

let middlewareList = [
		{ route: '/what', handle: (req, res, next) => res.send('what') },
    	{ route: '/hello', handle: (req, res, next) => res.send('hello') }
	];

let proxy = jsonCachingProxy({remoteServerUrl, middlewareList});

```

## License

MIT