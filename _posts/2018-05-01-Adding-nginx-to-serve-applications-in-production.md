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
