# Frontend service container

The first point is we need to recall the very basics for a web frontend client. A frontend client runs in a browser. That means the all the html, css and scripts needs to be loaded in the browser and the browser is the runtime environment for the web frontend client. Yes, that's sounds simple, but sometimes we tend to forget it.

Challenges are in that context:

* How to transfer the static files?
* How to secure the container during the execution?
* How to handle changing endpoints?

### How to transfer the static files?

Based on that fact we need a way to transfer all the `static` files for to browser as fast as possible over the internet.

Also we need to keep in mind we want to provide our frontend later as a container, so that our application can be executed on different container runtime platforms such as serverless Code Engine, Kubernetes, OpenShift and so on. 

With all this in mind we need to chose an effective web server, we have chosen the open source version of [Ngnix](http://nginx.org) do to that job.

### How to secure the container during the execution?

[Ngnix](http://nginx.org) is fast, but by default it needs to run as root when it listen to port 80 (additional information on [stackoverflow](https://unix.stackexchange.com/questions/134301/why-does-nginx-starts-process-as-root#:~:text=The%20reason%20this%20process%20is,under%20%2Fvar%2Frun%2F.)). So we configured the Docker container image to as non root. Our your user is called `nginxuser`.

This is an extract of the Dockerfile for the frontend application.

```dockerfile
# Add a user how will have the rights to change the files in code
RUN addgroup -g 1500 nginxusers 
RUN adduser --disabled-password -u 1501 nginxuser nginxusers
```

### How to handle changing endpoints?

The next topic is how to set variables for an application which doesn't run on the server you control? Because, when our container will start on different platforms the URLs for App ID and the backend will changing? 

The way to handle this is the externalization of variables, so that we can provide the container during the startup the needed information. How that works is one of the additional topics in our documentation. 

