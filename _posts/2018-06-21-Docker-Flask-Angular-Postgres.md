I spent more than 24 hours trying to figure out why a flask app is unable to communicate with a postgres sql database 
while both are shipping in docker containners.


docker exec -it containaer_id command


run inside a container 


fix the problem with docker port for postgres
**1rst Attempt :**

[from this ](https://stackoverflow.com/a/48140493/4683950)
answer on stackoverflow i need to check my databse url, the dabase is correct

change database uri to
ENV DATABASE_URL="postgresql://espoir_mur:9874@0.0.0.0:5432/adra_hr"

**2nd Attempt :**

[Try this answer](https://stackoverflow.com/a/47378186/4683950)
here it' said that i should Y run a container called dadarek/wait-for-dependencies as a mechanism to wait 
for services to be up(in your case, postgres).

but it doesn't works also.


keep debugging 
**3rd attempt **:

I go and read about race condition with the python app and the docker container [here](https://github.com/arachnys/cabot/issues/416)
I need to write a python script that check if the port is open , but the script doesn't work also.




