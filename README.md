Censorinus is a StatsD client with multiple personalities.

# Features

* All metric names and such are encoded as UTF-8
* Client-side sampling, i.e. don't send it to across the network to reduce traffic
* Asynchronous or Synchronous, your call!
* StatsD Compatibility

# Example

```scala
import github.gphat.censorinus.Client

val c = new Client()

// Optional sample rate, works with all methods!
c.counter(name = "foo.count", value = 2, sampleRate = 0.5)
c.increment(name = "foo.count") // Defaults to 1
c.decrement(name = "foo.count") // Defaults to 1
c.gauge(name = "foo.temperature", value = 84.0)
c.histogram(name = "foo.depth", value = 123.0)
c.meter(name = "foo.depth", value = 12.0)
```

# Asynchronous (default behavior)

Metrics are locally queued up and emptied out periodically. By default any
pending metrics are emptied out every 100ms. You can change this to another
delay:

```scala
val c = new Client(flushInterval = 50)
```

# Synchronous

If you instantiate the client with `asynchronous=false` then the various metric
methods will immediately emit your metric synchronously using the underlying
sending mechanism. This might be great for UDP but other backends may have
a high penalty!

# Sampling

All methods have a `sampleRate` parameter that will be used randomly determine
if the value should be enqueued and transmitted downstream. This lets you
decrease the rate at which metrics are sent and the work that the downstream
aggregation needs to do. Note that only the counter type natively understands
sample rate. Other types are lossy.

# StatsD Compatibility & Features

Censorinus is compatibile with the StatsD specification as defined [here](https://github.com/b/statsd_spec).
