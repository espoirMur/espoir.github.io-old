# Configure nginx to serve apps in production

For now i have a app with both flask , angular and a postgres database running inside a dockers containers and communicating well.

The next step is to serve all those app using nginx in production, but how??

I was told me to use nginx server.
but don' have any clue about nginx and how it works .
so let me understand how it works and try to make it work for me .
hope will not send many hours on this as i do in the past parts.

- 1 the flask app


# the flask app should be serve by wsgi

following [this](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-14-04) tuto

create a wsqgi file and run the application like this :

```python
from run import app as application


if __name__ == "__main__":
    application.run(threaded=True, debug=True)

```

Note that I need to import app as applicaation and run it .
if it was jus import app it could not run.

Then we create .ini config file for the project;

Let's place that in our project directory and call it myproject.ini:

/Users/guillaumesayinzoga/espoir-stuff/adra-hr-application/api



nginx is serving the angular files.

https://stackoverflow.com/a/45297119/4683950



https://dzone.com/articles/dockerizing-angular-app-via-nginxsnippet




 
 https://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html#deploying-django
 
 
 2018/06/22 16:06:45 [emerg] 1#1: "server" directive is not allowed here in /etc/nginx/nginx.conf:1
 
 uwsgi: unrecognized option '--chdir api/'
 
 #for the nginx with angular it working now..
 but how to work the uwsgi with python??
 
 I run into the following error :
 to start the wsgi file from parent directory

 sudo uwsgi --chdir api/ --ini api/api.ini
 
 nothing was fixed .
 ***went back to the uwsgi doc **
 [here](http://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html)
 
 read more and understand how the uwsgi works and was able to configure the project well.
 
uwsgi is an application server for hosting python application for production.

The uWSGI project aims at developing a full stack for building hosting services.

Application servers (for various programming languages and protocols), proxies, process managers and monitors are all implemented using a common api and a common configuration style.


here is what is said in his documentation.

*The uWSGI project aims at developing a full stack for building hosting services.
Application servers (for various programming languages and protocols), proxies, process managers and monitors are all implemented using a common api and a common configuration style.*

now we need to configure it to host our application.

here is how our configuration look like 

```
[uwsgi]
http-socket = 0.0.0.0:8000
#for the docker configuration
chdir = /api/api
wsgi-file = wsgi.py
callable = application
processes = 4
threads = 2
stats = 0.0.0.0:9191

```
 
the first lign tell to use http-socket at the localhost with the port.
we are not going to use http because we are serving our frontend by another server.

chdir : tell the working directory of our project
wsgi : to specifie  our wsgi file
the callable is our application imported in the first lign of wsgi.py 
the application is runned in 2 threads of 4 processes.

for ssl and https i should read [this](http://nginx.org/en/docs/http/configuring_https_servers.html)



and now [the following tutorial](https://realpython.com/dockerizing-flask-with-compose-and-machine-from-localhost-to-the-cloud/) gives step by step instruction to deploy to digital ocean 


I managed to mak it work but once in digital ocean i start getting 
502 errros bad gateway:

How to fix it?
https://stackoverflow.com/a/47091862/4683950

in nginx.conf

server name and proxy pass should be the same

remove the dns config

okey ! the next day i decide to learn how nginx config works on digital ocean.
started with [this](https://www.digitalocean.com/community/tutorials/understanding-the-nginx-configuration-file-structure-and-configuration-contexts)


so my nginx.config should look like this .

```
http {

    # http context

    server {

        # first server context

    }

 http {

    # http context

    server {

        # first server context

    }

    server {

        # second server context

    }

}


}
```

then going to add the following ligns in the sever block.

- **listen:** The ip address / port combination that this server block is designed to respond to. 
If a request is made by a client that matches these values, this block will potentially be selected to handle the connection.

- **server_name:** This directive is the other component used to select a server block for processing.
If there are multiple server blocks with listen directives of the same specificity that can handle the request,
Nginx will parse the "Host" header of the request and match it against this directive.

so in listen i have to put the ip adress of my digital ocean dyno and the port 80 where nginx is running.
listen         139.59.162.16:80;

NB : i noticed that i  had 2 listen blocks in my config, may be that is why it was not working.
so let kee investigating?

**serv_name** it not understood yet, still confusing.


The next is the location context.
this is what is said there :

_each location is used to handle a certain type of client request, 
and each location is selected by virtue of matching the location definition against the client request through a selection algorithm._


here are others important information about server and location.


While server contexts are selected based on the requested IP address/port combination and the host name in the "Host" header, 
location blocks further divide up the request handling within a server block by looking at the request URI. 
The request URI is the portion of the request that comes after the domain name or IP address/port combination.

So, if a client requests http://www.example.com/blog on port 80, the http, www.example.com, and port 80 would all be used to determine which server block to select. After a server is selected, the /blog portion (the request URI), would be evaluated against the defined locations to determine which further context should be used to respond to the request.

acccording to that information here is how my .config file looks like
```
  location / {
              proxy_pass http://139.59.162.16:8080;
              include  /etc/nginx/mime.types;
              root   /usr/share/nginx/html;
               index  index.html index.htm;
           }
  #all request staring with adra and api are passed to the backend
           location /api {
                 proxy_pass          http://0.0.0.0:8000;
                 proxy_set_header        Host $host;
           }

           location /adra {
                 proxy_pass          http://0.0.0.0:8000;
                 proxy_set_header        Host $host;
           }
           # to load css and javascript
           location ~ \.css {
               add_header  Content-Type    text/css;
           }
           location ~ \.js {
               add_header  Content-Type    application/x-javascript;
           }

```

I have 4 location; 

the 2 with both /api , /adra are related to the api in the flask file

the second location I added them when I was trying to solve a problem of css files they was not read , and was served as text files.

Now need to understedn th proxy_pass thing , may be that is why I'm getting the 502 error.

[Here](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass)

__Sets the protocol and address of a proxied server and an optional URI to which a location should be mapped. As a protocol, “http” or “https” can be specified. The address can be specified as a domain name or IP address, and an optional port:__


for the docker and nginx i head about the reverse proxy thing .


found this recent question I'm sure it give the answer to my problem.

__For those that weren't aware, whenever you use docker-compose up, it automatically creates a default docker network if you don't specify any in the docker-compose.yaml. Docker containers in the same network automatically get a DNS entry created for the container name. 
That means you can access the other containers in the network using something like http://mycontainername:8080.__

it means that in the proxy pass should be the adress on which you app are acccesible inside the docker.

for my case for the flask app I change it to  http://api:8000 and for nginx it need to be changed to :
http://nginx_demo:8080

or the ip adress of the server host


Was using the wrong machine ip adress .

now 
getting 502 error acces not authorise:
removed the proxy_pass in the location /

gettinng 403 error adding right file right

now files are not served .

the problem is with nginx it's looking for file in /etc/nginx/
while all my file are in 

/usr/share/nginx/html


attend to find the solution here by changing the way files are server in .config

https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/



  
