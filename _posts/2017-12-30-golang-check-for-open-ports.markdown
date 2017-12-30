---
layout: post
title:  "Checking for open ports in go lang"
date:   2017-12-30 00:00:00 +0000
categories: go-lang
---
I had to create a periodic status check for an open port on a specific host for a service I was creating recently. The status check has to work for both the localhost in development and also on the remote host.

Using the [net][net pkg] package in __Go__ I was able to come up with the following snippet for testing the localhost port:

{% highlight go %}
package main

import (
  "fmt"
  "net"
)

func main() {
  l, err := net.Listen("tcp", ":" + port)
  defer l.Close()

  if err != nil {
    // Log or report the error here
  }
}
{% endhighlight %}

We use [Listen][net pkg Listen] above as it works for localhost only.

For testing the remote port, we can use [DialTimeout][net pkg DialTimeout] as it accepts a custom timeout parameter:

{% highlight go %}
package main

import (
  "fmt"
  "net"
  "time"
)

func main() {
  // host -> the remote host
  // timeoutSecs -> the timeout value
  conn, err := net.DialTimeout("tcp", host, time.Duration(timeoutSecs)*time.Second)
  defer conn.Close()

  if err != nil {
    // Log or report the error here
  }
}
{% endhighlight %}

[net pkg]: https://golang.org/pkg/net/
[net pkg Listen]: https://golang.org/pkg/net/#Listen
[net pkg DialTimeout]: https://golang.org/pkg/net/#DialTimeout
