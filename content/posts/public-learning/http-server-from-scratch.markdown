---
layout: post
title: "Creating an HTTP Server from Scratch in C++"
date: 2021-11-09T18:22:39-07:00
description: Creating an HTTP Server From Scratch in C++.
draft: true
---

## Intro
While I have no real need to create an HTTP server from scratch, one of the best ways to learn how something works is to implement it yourself, thus this article was born! 

I am going to go through my understanding of HTTP and implement a simple server within C++. I am going to do my best document resources and reading that were referenced while writing this post.

## What is HTTP?
The best place to find a summarized explanation of HTTP is on the Mozilla website linked [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview).

Hyper Text Transfer Protocol (HTTP) is a data transfer protocol for sending data over the internet through a standardized and extensible message format. There are three distinct parts to these messages:
1. **Start line**: contains information such as the type of request, the requested data, the protocol, and the response status code
2. **Headers**: contains meta data about the request or response
3. **Body**: contains additional data about the request from the client or the response data from the server

More information on the message format can be found [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages).


### Examples
Below are examples of request and response HTTP messages for a `GET` request to `jabasco.dev` on my current machine. What we are looking for in these messages are a single start line, a variable number of header parameters, and a blank line followed by the body:

{{< highlight go "hl_lines=2 9" >}}
*****REQUEST*****
GET / HTTP/2
Host: jabasco.dev
User-Agent: Mozilla/5.0 Gecko/20100101 Firefox/94.0
Accept: text/html
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br

 
{{< / highlight >}}

{{< highlight go "hl_lines=2 7-11" >}}
*****RESPONSE*****
HTTP/2 200 OK
server: Netlify
vary: Accept-Encoding
content-length: 2068

<!DOCTYPE html>
<html>
...
</html>
{{< / highlight >}}

What is pretty apparent when looking at these messages is that they extremely human readable and extensible which are huge strengths of HTTP.

### Transfer Process
https://networkengineering.stackexchange.com/questions/6007/how-http-is-converted-to-tcp-and-then-how-tcp-converted-to-ip
1. Create the HTTP packet
2. Encapsulate the HTTP packet within a TCP packet
3. Encapsulate the TCP packet within an IP packet
4. Send these packets to the server and then back out to the HTTP packet
5. Do the same process from server to client