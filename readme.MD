# Request Throttler Express
A simple request rate throttler for node and express applications.
<h1>
<img src="https://api.travis-ci.org/asif-ir/request-throttler-express.svg?branch=master"/>
<img src="https://img.shields.io/npm/v/request-throttler-express.svg?style=flat-square"/>
<img src="https://img.shields.io/david/asif-ir/request-throttler-express.svg?style=flat-square"/>
</h1>

## Installation
The following command will install and set it up for your current project:

``` bash
    npm install request-throttler-express
```

Currently you need to have a redis server running for using this module. More caching stores will be added soon.

## Usage
Request throttler can be used just as any other standard express.js middleware.

``` javascript
    const requestThrottler = require('request-throttler-express');
    
    // Register as a middleware
    app.use(requestThrottler.redis());
```

By default, it will limit to 10 requests per minute. To take more control, check out the possible options you can pass
to the middleware below.

For a general case though, you might want to pass some connection details to your redis server. The above middleware
also accepts an optional object where you can pass connection details within a connection property as under:
  
``` javascript
    app.use(requestThrottler.redis({
        connection: {
            host: 'localhost',
            port: 6379,
        }
    }));
```
For more knowledge on what can be passed in the connection property, please go through the redis 
documentation [here](https://www.npmjs.com/package/redis).

## Options
The middleware function accepts a variety of optional parameters.

-  **ttl** \
    The size of the time window in seconds(time to live). Default value is _60_.
    
-  **maxHits** \
    The maximum number of requests that can be made from any particular IP in a given window. Default value is _10_.

-  **ipGetter** \
    This parameter accepts a function with the request object as the argument. It expects the user to return the ip 
    address as a string. The default implementation retrieves the standard express _req.ip_.
    
-  **throttleHandler** \
    This parameter accepts a function with the request object as the argument. The user can do logging and similar 
    things based on their requirements here. By default, it's a no-op.
        
-  **errorHandler** \
  This parameter accepts a function with the err object as the argument. The user can handle caching store connection
  failures here. By default, it logs the error on the console.
  
## Example Usage
The options can be used in any combination like following:

``` javascript
    app.use(requestThrottler.redis({
        ttl: 600,
        maxHits: 20,
    }));
```

## License
This library is freely distributable under the terms of the [MIT license](https://github.com/asif-ir/request-throttler-express/blob/master/LICENSE). (MIT)

