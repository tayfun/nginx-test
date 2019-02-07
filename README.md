Proxying using Nginx
====================

Clone the repo to get the nginx conf and use docker to test this out:

    docker build -t alpine-nginx-lua .

    docker run --rm -p 1234:80 -v /absolute/path/to/repo/nginx/nginx.conf:/etc/nginx/nginx.conf:ro alpine-nginx-lua

And open up http://localhost:1234 on the browser. Refresh the page a few times while making sure you're still on localhost and thus hitting your local Nginx. This is because the remote servers you are proxying can redirect you to elsewhere.

Nginx will alternate between proxying the request to BBC and to Google (roundrobin is the default method). If the servers for those names (as in IP addresses for their domains in the config file, in our case BBC and Google) do not respond, you will get an error. This can be because of the `Host` header for example. If you use bbc.co.uk as a proxied domain for example, you may get fastly error as fastly requires strict `Host` value (likely because they host configurations for lots of domains and thus need the domain to pick the config they will use). With the Lua dynamically choosing an upstream in this commit, `host` is correctly set.

So one of the problems this was testing for was for AppEngine. The problem with proxying for AppEngine would be similar but on a bigger scale. I'd imagine AppEngine boxes hosting multiple apps and the way to differentiate which one to route to would be the Host header.
