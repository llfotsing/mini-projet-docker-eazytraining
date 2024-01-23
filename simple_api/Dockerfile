FROM python:3.8-buster 
# maintainer information
LABEL Landry FOTSING <landrycfotsing@gmail.com>
# Set the working directory
WORKDIR /
# Copy the source code of the API to build the image
COPY student_age.py /
# Installation of  prerequits
RUN apt-get update -y 
RUN apt-get install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y 
RUN  pip install flask==1.1.2 flask_httpauth==4.1.0 flask_simpleldap python-dotenv==0.14.0
VOLUME /data
EXPOSE 5000
ENTRYPOINT ["python"]
CMD ["./student_age.py"]
