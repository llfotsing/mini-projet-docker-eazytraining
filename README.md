# student-list 
This repo is a simple application to list student with a webserver (PHP) and API (Flask)

![project](https://user-images.githubusercontent.com/18481009/84582395-ba230b00-adeb-11ea-9453-22ed1be7e268.jpg)


------------


Firstname : Landry

Surname : FOTSING

For Eazytraining's 17th DevOps Bootcamp

Period : Janvier-Mars 2024

Linkedln : https://www.linkedin.com/in/landry-fotsing-55ba181a0/
 

## Application


We have an application "*student_list*" build by POZOS (an IT company) which is running on a single server with any scalability and any high availability. The application  enables POZOS to show the list of the student with their age.

The goal is to ensure that the existing infrastructure can be scalable, easily deployed with a maximum of automation.

student_list has two modules:

- the first module is a REST API (with basic authentication needed) who send the desire list of the student based on JSON file
- The second module is a web app written in HTML + PHP who enable end-user to get a list of students

my work is to build one container for each module an make them interact with each other.


```bash 
 $url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages';
 ```


**Congratulation! Now you are ready for the next step (docker-compose.yml)**

## my work

like i said early, my work is to build one container for each module an make them interact with each other.

Firstly, I will introduce to you the six files of this project and their role

Then, I'll show you how I built and tested the architecture

## Project files

Here are the files that you will find in my delivery. I listed them with their role.

- docker-compose.yml: to launch the application (API and web app)
- simple_api/Dockerfile: to build the API image with the source code in it
- simple_api/student_age.py: contains the source code of the API in python
- simple_api/student_age.json: contains student name with age on JSON format
- index.php: PHP  page where end-user will be connected to interact with the service to list students with their age.

## Build and test

Considering you just have cloned this repository, you have to follow those steps to get the 'student_list' application ready :
1. go to the project directory and build the api container image :

```
cd ./mini-projet-docker-eazytraining/simple_api
docker build . -t api.student_list
docker images
 ```
![Uploading image.jpg…]()

2. 
  
## Infrastructure As Code (5 points)

After testing your API image, you need to put all together and deploy it, using docker-compose.

The ***docker-compose.yml*** file will deploy two services :

- website: the end-user interface with the following characteristics
   - image: php:apache
   - environment: you will provide the USERNAME and PASSWORD to enable the web app to access the API through authentication
   - volumes: to avoid php:apache image run with the default website, we will bind the website given by POZOS to use. You must have something like
`./website:/var/www/html`
   - depend on: you need to make sure that the API will start first before the website
   - port: do not forget to expose the port
- API: the image builded before should be used with the following specification
   - image: the name of the image builded previously
   - volumes: You will mount student_age.json file in /data/student_age.json
   - port: don't forget to expose the port
   - networks: don't forget to add specific network for your project

Delete your previous created container

Run your docker-compose.yml

Finally, reach your website and click on the bouton "List Student"

**If the list of the student appears, you are successfully dockerizing the POZOS application! Congratulation (make a screenshot)**

## Docker Registry (4 points)

POZOS need you to deploy a private registry and store the built images

So you need to deploy :

- a docker [registry](https://docs.docker.com/registry/ "registry")
- a web [interface](https://hub.docker.com/r/joxit/docker-registry-ui/ "interface") to see the pushed image as a container

Or you can use [Portus](http://port.us.org/ "Portus") to run both

Don't forget to push your image on your private registry and show them in your delivery.

## Delivery (4 points)

Your delivery must be zip named firstname.zip (replace firstname by your own) that contain:

- A doc or PDF file with your screenshots and explanations.
- Configuration files used to realize the graded exercise (docker-compose.yml and Dockerfile).

Your delivery will be evaluated on:

- Explanations quality
- Screenshots quality (relevance, visibility)
- Presentation quality

Send your delivery at ***eazytrainingfr@gmail.com*** and we will provide you the link of the solution.

![good luck](https://user-images.githubusercontent.com/18481009/84582398-cad38100-adeb-11ea-95e3-2a9d4c0d5437.gif)
