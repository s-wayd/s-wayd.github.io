# Docker Project
NOTE: I attempted to accomplish this assignment on my MacBook. However, once I reached the end of the project, a strange error (which I looked up on the web and in numerous example codes) did not allow me to continue with the installation. 

I completed this assignment using the borrowed DELL PC computer Isec-LPT-23.

## Step 1: Install Docker Desktop for Mac
This was completed  by downloading Docker and then opening it.

## Step 2: Prepare Dictionary
In the borrowed laptop terminal, I typed in the following code:

    mkdir -p wordpress
    cd wordpress

## Step 3: Create the YAML File
I then I used notepad to create a file with the yaml format. 

    docker-compose.yaml

After it opened a file in vim where I entered in the following information:

    version: '3'
    services:
        mysql:
            image: mysql:latest
            restart: always
            environment:
                MYSQL_ROOT_PASSWORD: my_password
                MYSQL_DATABASE: wordpress
                MYSQL_USER: wordpress_user
                MYSQL_PASSWORD: wordpress_password
            volumes:
                - mysql_data:/var/lib/mysql
        wordpress:
            image: wordpress:latest
            depends_on:
                - mysql
            ports:
                - 8080:80
            restart: always
            environment:
                WORDPRESS_DB_HOST: mysql:3306
                WORDPRESS_DB_USER: wordpress_user
                WORDPRESS_DB_PASSWORD: wordpress_password
            volumes:
                - ./wp-content:/var/www/html/wp-content
    volumes:
        mysql_data:

## Step 4: Run WordPress with Docker Compose
type in:

    sudo docker-compose up -d

    