ssh2-multiplexer
================

* SSH2 exec channel multiplexer.

* Allows to multiplex the available SSH channels, in order to exec a larger number of commands.


### Example

``` js
var Connection = require('ssh2'),
  ConectionPool = require('ssh2-multiplexer'),

var conn = new Connection();
//...
var pool = new ConnectionPool(conn);
pool.exec('uptime', function(err, stream) {
  if (err) throw err;
  stream.on('data', function(data, extended) {
    console.log((extended === 'stderr' ? 'STDERR: ' : 'STDOUT: ') + data);
  });
  stream.on('end', function() {
    console.log('Stream :: EOF');
  });
  stream.on('close', function() {
    console.log('Stream :: close');
  });
  stream.on('exit', function(code, signal) {
    console.log('Stream :: exit :: code: ' + code + ', signal: ' + signal);
    conn.end();
  });
});
```

