Proxying using Nginx
====================

Clone the repo to get the nginx conf and use docker to test this out:

    docker run --rm -p 1234:80 -v /absolute/path/to/repo/nginx/nginx.conf:/etc/nginx/nginx.conf:ro nginx:mainline-alpine

And open up http://localhost:1234 on the browser. Nginx will alternate between proxying the request to Apple and to Google. If the servers for those names (as in IP addresses for their domains in the config file) do not respond you will get an error. This can be because of the `Host` header for example. I've set this to empty string so as to not confuse the remote servers (I think they are more likely to respond when it is empty than when it is wrong, such as app1). If you use bbc.co.uk as a proxied domain for example, you will get fastly error as fastly requires strict `Host` value (likely because they host configurations for lots of domains and thus need the domain to pick the config they will use).
