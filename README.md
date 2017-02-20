# rloris

`rloris` is an implementation of RSnake's SlowLoris attack, along with some other attacks, in Rust.
Currently it only supports SlowLoris - other attacks are coming!

You can download the source code and build it - you'll need: 
* the OpenSSL or LibreSSL dev headers
* a working Rust compiler, stable/beta/nightly all work
* `pkg-config`

You can also download a working binary for Linux x64 from the [releases directory](https://github.com/SilverWingedSeraph/rloris/releases).

## Attacks

* SlowLoris GET, based on [RSnake's original attack](https://web.archive.org/web/20090822001255/http://ha.ckers.org/slowloris/), simply opens a lot of connections
    asking to GET resources and then takes its time sending the other headers and the final double `\r\n`. This is the default.
* SlowLoris POST, similar to the above but using POST requests. Activate with `--post`.

## Functionality

Optional functionality is available by giving flags to the `rloris` binary. Currently, `rloris` supports SSL (`-ssl`), 
setting custom timeout and cycle counts (`--timeout=<timeout>` and `--cycle=<cycles>`),
multithreading (`--thread=<threads>`), and infinite repetition (`--repeat`). 

WARNING: using `--repeat` can create a real honest to goodness DoS condition on the target!

## Advice

The more threads you can get away with, the higher the impact on the target. To see what your server can handle, 
keep raising the number of threads until you get "Connection reset by peer" errors; at that point, your server is dropping connections due to over-load.
Note that a DoS condition probably will arise before this happens.

### Examples

* Perform an attack in plaintext against localhost: `rloris localhost`
* Perform an attack against localhost, port 8000, using the POST verb: `rloris localhost --port=8000 --post`
* Perform an SSL attack against example.com, port 443: `rloris example.com --ssl`
* Perform an SSL attack against 127.0.0.1, with domain name example.com: `rloris 127.0.0.1 --ssl --domain=example.com`
* Perform a repeated attack against localhost: `rloris localhost --repeat`
* Perform a parallelized attack against localhost: `rloris localhost --threads 4`