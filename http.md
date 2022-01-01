# HTTP

The Hypertext Transfer Protocol (HTTP) is an application protocol for
distributed, collaborative, hypermedia information systems. HTTP is the
foundation of data communication for the World Wide Web, where hypertext
documents include hyperlinks to other resources that the user can easily access,
for example by a mouse click or by tapping the screen in a web browser.

## HTTP server

HTTP server one liner with busybox

```sh
$ busybox httpd -f -p 8000
```

or for port 80:

```sh
$ busybox httpd -f -p 80
```
