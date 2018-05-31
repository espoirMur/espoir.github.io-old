# Fundemental of IOT session

I finally landed to Nyeri after a long trip of more than 1000 km by us .I think I should write a blog post the trip too.

But let me talk about serious things first.

Later that year i applied to the data science summer school which is organized with data science africa and I had the privelge to
be among the few people who was selected to attend the event.
The summer school is a workshop where suddent are trained about data science and their pratical application.
you can fouund more about the 2018 event [here](http://www.datascienceafrica.org/dsa2018/).

The theme of this year workshop is : _end-to-end data science._ .

The day one start with a brief introduction to data science done by : 
**Dina Machuve**  from  Nelson Mandela African Institute of Science and Technology
she gaves us a short introduction on data science and explain us the difference between Machine learning and data science(the famous debate)

The Good thing started when **Jan Jongboom** from ARM started his lecture about **Fundamentals of IoT**
i was still wondering what IOT has to do with data science and machine learning but he gives me clarification when he said that :

__Playing with machine learning models is fun but...
but data acquisition is even more important__

Today we are going to learn how to collect data with sensor .

He talked us about ARM devices for data collection, 
we learned about the ARM MBED micro controller, loaWAN connectivity kits and some sensor used by ARM?

We started pratical session , and it was about to set up account on arm site and make some simullation with the ARM (micro controller  which was pretty to arduino 
micro controller we have used at school).

Before the break my kit was ready and was able to collect noisure data .

(Put the picture here)

After the break we went to the field collecting data in real live.

We deployed the sensors in the tomatoes greenhosue .
We use a kit with 3 sensors temperature sensor, moisure sensor and humidity sensor, and lorawifi shield, we deployed them and started collected the data .

The data was directly being saved in an ElasticSearch database and visualised via platfeform (In the comming days data engineers from ARM will tell us how the whole process of collecting data from sensor  to kibana plateform will be explained tommorow.)
