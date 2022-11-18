# Docker wordpress guide
## obtaining docker
run the following code to obtain docker 
```
sudo apt update
sudo apt install docker.io docker-compose-plugin
```
you can test if docker is installed correctly with the following commmand
`sudo docker run hello-world`
## wordpress
next create a directory for wordpress and then make that your pwd with
``` 
    mkdir wordpress
    cd wordpress
```
make a yml file with docker compose such as with nano with the following command
`nano  docker-compose.yml`
then paste the following into the file
```version: "3" 
# Defines which compose version to use
services:
  # Services line define which Docker images to run. In this case, it will be MySQL server and WordPress image.
  db:
    image: mysql:5.7
    # image: mysql:5.7 indicates the MySQL database container image from Docker Hub used in this installation.
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: MyR00tMySQLPa$$5w0rD
      MYSQL_DATABASE: MyWordPressDatabaseName
      MYSQL_USER: MyWordPressUser
      MYSQL_PASSWORD: Pa$$5w0rD
      # Previous four lines define the main variables needed for the MySQL container to work: database, database username, database user password, and the MySQL root password.
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    restart: always
    # Restart line controls the restart mode, meaning if the container stops running for any reason, it will restart the process immediately.
    ports:
      - "8000:80"
      # The previous line defines the port that the WordPress container will use. After successful installation, the full path will look like this: http://localhost:8000
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: MyWordPressUser
      WORDPRESS_DB_PASSWORD: Pa$$5w0rD
      WORDPRESS_DB_NAME: MyWordPressDatabaseName
# Similar to MySQL image variables, the last four lines define the main variables needed for the WordPress container to work properly with the MySQL container.
    volumes:
      ["./:/var/www/html"]
volumes:
  mysql: {}
```
# IMPORTANT this is a template, replace the usernames and passwords with corresponding info
next run the container with the following command
` sudo docker compose up -d`
Note: i had issues connecting to the docker repository when i ran this command, so if you have issues keep trying the command.

## configuring wordpress in the web browser
 it is important to note that doing this in my VM when trying to connect to the localhost you must use them machines ip connection and not the local machine.
 for instance my vm is connected through  10.10.1.105 so i needed to connect through 10.10.1.105:8000
 trying to use my machines localhost will give me 127.0.0.1:8000 which will fail to connect

 Next go to the localhost setup in the yml file in my case 10.10.1.105:8000

Follow the on screen prompts to setup your website ensuring you setup a username and secure password you will remember and voila! you have your own wordpress site
![Alt text](../../../../C:/Users/Prinak/Pictures/wordpress_site_proof.PNG)

### guides used
I used a guide from the following website  [hostinger.com](https://www.hostinger.com/tutorials/run-docker-wordpress)

I modified their guide to create this writeup to alleviate the problems i encoutered in setting this up for myself