# Configure nginx to serve apps in production

For now i have a app with both flask , angular and a postgres database running inside a dockers containers and communicating well.

The next step is to serve all those app using nginx in production, but how??

My supervisor told me to use nginx server.
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



to start the wsgi file from parent directory

 sudo uwsgi --chdir api/ --ini api/api.ini
 
 https://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html#deploying-django
 
 
 2018/06/22 16:06:45 [emerg] 1#1: "server" directive is not allowed here in /etc/nginx/nginx.conf:1
 
 uwsgi: unrecognized option '--chdir api/'
 
 
