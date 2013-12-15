# cogent [![Build Status](https://travis-ci.org/cojs/cogent.png)](https://travis-ci.org/cojs/cogent)

A simple HTTP request agent designed primarily for `GET`ing stuff.

## Features

- Resolve redirects
- Automatic gunzipping
- First-class JSON support
- Buffer the response or save it to a file

## API

```js
var request = require('cogent')
```

### var response = yield* request(url, [options])

`url` is the URL of the request.
The options are passed to [http.request()](http://nodejs.org/api/http.html#http_http_request_options_callback).
Additional options are:

- `buffer` - buffer the response and save it as `res.buffer`
- `string` - buffer the response as a string and save it as `res.text`
- `json` - buffer the response as an object and save it as `res.body`
- `destination` - cojs/cogent the response to the file `destination`

If `options === true`, it defaults to `{ json: true }`.
If `typeof options === 'string'`, it defaults to `{ destination: string }`.

`response` will have the following properties:

- `res.req` - the request object
- `res.res` - if the response was gunzipped, it'll return the gunzip stream.
  The original response object will be stored as `res.res`.
- `res.statusCode`
- `res.headers`
- `res.destination` - populated only if the file was successfully saved on a `200`
- `res.buffer`
- `res.text`
- `res.body` - JSON body populated only on a `200`

```js
var uri = 'https://raw.github.com/visionmedia/express/master/package.json'

// Save to a file
var res = yield* request(uri, require('os').tmpdir() + '/express.package.json')
if (res.destination) console.log('ok')

// Get as JSON
var res = yield* request(uri, true)
var json = res.body
```

## License

The MIT License (MIT)

Copyright (c) 2013 Jonathan Ong me@jongleberry.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.