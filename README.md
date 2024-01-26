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
![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/8dfaa05d-e454-4502-b8c2-4d9f90cc4027)


2. Create a bridge-type network for the two containers to be able to contact each other by their names thanks to dns functions :

```
docker network create student_list.network --driver=bridge
docker network ls
```
![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/d74c54e3-1f8d-4a4d-9956-6f831640f089)

3. Move back to the main project directory and run the backend api container:

```
cd ..
docker run --rm -d --name=api.student_list --network=student_list.network -v ./simple_api/:/data/ api.student_list
docker ps
```
![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/54eed2e5-53f4-4e15-9586-f6e546759cb1)


The api backend container is listening to the 5000 port. This internal port can be reached by another container from the same network.

4. Update the index.php file

You should to update the following line in the index.php file before running the website container to make api_ip_or_name and port fit your deployment ``$url = http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages``.

The line should become ``$url = http://api.student_list:5000/pozos/api/v1.0/get_student_ages``

5. Run the frontend webapp container

```
docker run --rm -d --name=webapp.student_list -p 80:80 --network=student_list.network -v ./website/:/var/www/html -e USERNAME=toto -e PASSWORD=python php:apache
docker ps
```
![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/105c8040-fbc3-42ec-b042-dcf434d9c5a7)

![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/644655e9-a6d1-4ec0-a9f9-de4045ea23cc)

6. Test the api through the frontend

- **Test using command line**
  
  The next command will ask the frontend container to request the backend api and show you the output back.
  ```
  docker exec webapp.student_list curl -u toto:python -X GET http://api.student_list:5000/pozos/api/v1.0/get_student_ages
  ```

  ![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/3f244545-eab5-4e5b-b201-ac36fe46b5e4)

- **Test using a web browser**

  You can get your ip-adress by using the following command: ``hostname -i``

  If you are working on Playwithdocker like me, just ``open the 80 port``

  ![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/354ecb45-be50-42d3-a849-b64a46797103)

  ![image](https://github.com/llfotsing/mini-projet-docker-eazytraining/assets/98328155/b4fe505f-b2c2-40a7-b3bf-790b62b70cdf)

7. Clean the workspace

```
docker stop api.student_list
docker stop webapp.student_list
docker network rm student_list.network
docker network ls
docker ps
```
