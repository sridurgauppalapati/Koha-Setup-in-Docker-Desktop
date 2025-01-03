![image](https://github.com/user-attachments/assets/87cc4794-3971-48ba-9cb0-63b4d3b0c92f)# Koha-Setup-in-Docker-Desktop
Setup Koha Application in Docker Desktop
Setting Up Koha with Docker: A Comprehensive Guide
# Introduction
In the world of library management systems, Koha stands out as a powerful, open-source solution that has empowered libraries globally. For those eager to streamline its setup using Docker, this article provides a comprehensive, step-by-step guide. Whether you're a tech enthusiast or a library professional, this guide is crafted to simplify the process and get you up and running with Koha in no time!

# Why Use Docker for Koha?
Docker containers offer a consistent, lightweight, and flexible environment for setting up applications. With Docker, you can deploy Koha in a controlled setup without worrying about system-level dependencies.

# Step 1: Preparing the Dockerfile
To begin, create a Dockerfile and populate it with the following commands:

# dockerfile

FROM debian:bullseye  
RUN apt update  
RUN apt install -y sudo apt-transport-https ca-certificates curl vim wget systemd  
RUN sudo mkdir -p --mode=0755 /etc/apt/keyrings  
RUN curl -fsSL https://debian.koha-community.org/koha/gpg.asc -o /etc/apt/keyrings/koha.asc  
RUN sudo apt update  
RUN echo 'deb [signed-by=/etc/apt/keyrings/koha.asc] https://debian.koha-community.org/koha 24.05 main' | sudo tee /etc/apt/sources.list.d/koha.list  
RUN sudo apt update  
RUN sudo apt install -y koha-common  
RUN sudo apt install -y mariadb-server    
RUN sudo a2enmod rewrite  
RUN sudo a2enmod cgi  
Save the Dockerfile and proceed to the next step.

# Step 2: Building and Running the Docker Container
Run the following command to build and launch the Docker container:

docker build -t koha-setup .  
docker run --privileged -p <port1>:<port1> -p <port2>:<port2> -it koha-setup /bin/bash  
Once the container is running, you'll be dropped into the root shell.

# Step 3: Post-Setup Commands
Inside the container, execute the following commands to complete the setup:

**1. Verify and Restart Apache:**

service apache2 status  
service apache2 restart  
**2. Edit Koha Configuration:**
Open the configuration file:

vi /etc/koha/koha-sites.conf  
Update the INTRAPORT with the port specified in the docker run command (e.g., 8080).

**3. Handle vi Installation Issues:**
If you encounter issues using vi, install it with:
apt install vim

**4. Set Up MariaDB:**
service mariadb status  
sudo koha-create --create-db <libraryname>  
sudo a2enmod headers proxy_http 

**5.Enable and Start Koha-Plack:**
sudo koha-plack --enable <libraryname>  
sudo koha-plack --start <libraryname>  

**6. Update Apache Ports:**
Open the ports configuration file:
vi /etc/apache2/ports.conf  

Add the following line:
Listen <port>  

Disable the default Apache site:
sudo a2dissite 000-default  

**7. Optimize Apache:**
sudo a2enmod deflate  
a2ensite <libraryname>  
service apache2 restart  

**8. Set Koha Password:**
Secure your Koha instance with:

sudo koha-passwd <libraryname>  

Now you can able to see below page when you try localhost:<port>
login using newly created username "koha_<libraryname>" and password generated
![image](https://github.com/user-attachments/assets/a67e531f-3f49-47c9-85a8-05ef355413f0)

Next , we need to complete below web installations and on-boarding 
![image](https://github.com/user-attachments/assets/fe531e16-a90c-43ad-b0a4-4cd0dce0bc15)

Login with newly created admin username and password
![image](https://github.com/user-attachments/assets/2b9833a8-b1b0-4018-9397-58c11ce658a0)


For API related information, Koha provided "https://api.koha-community.org/"



