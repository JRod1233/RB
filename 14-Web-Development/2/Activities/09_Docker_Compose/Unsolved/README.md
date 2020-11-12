## Activity File: Deploying and Testing a Container Set

In this activity, you will play the role of a DevOps engineer overseeing the process of standing up a container set for your company using Docker Compose. 

- The Risk Management team has approved the usage of the WordPress container in the cloud infrastructure.

- You are tasked with locally deploying multiple containers with Docker Compose and running tests to verify the functionality of the containers. You will also need to edit the Docker Compose file and test the results.


### Instructions

1. First, deploy the microservices:

    - Log onto the terminal environment and run the command: `sudo wget tinyurl.com/14-2--setup`

    - Make the downloaded file executable: `sudo chmod +x 14-2--setup`
    
    - Run the following script: `sudo ./14-2--setup`

    - Or in one command: `sudo wget tinyurl.com/14-2--setup && sudo chmod +x 14-2--setup && sudo ./14-2--setup`

2. Complete the following:

    - Navigate to your `/home/sysadmin/Cybersecurity-Lesson-Plans/14-Web_Dev/deploying_testing_activity` directory. 

    - Deploy the WordPress stack with `sudo docker-compose up`.
    
    - Verify the site is running by navigating to `192.168.2.2` in the browser.

3. In a new terminal window, verify that the application runs properly by completing the following:

    - Use an interactive bash session to enter the `db` container.

    - Verify that the MySQL credentials provided in the `docker-compose.yml` file are functional with `mysql -u [username] -p[password]`

       **Note:** You don't need to do anything further with the database, you just need to verify that the database and credentials work.

    - Exit the MySQL session, exit the interactive bash session, and press Ctrl+C to stop the Docker Compose stack.

4. Change the ports and redeploy:

    - Run `sudo docker-compose down` to clear the current configurations.

    - Edit the YAML file so that the first `ui1` service will use the IP address: `192.168.2.200`.

    - Redeploy the stack and check that your configuration changes worked with the browser.


#### Docker-Compose YAML

A copy of the `docker-compose.yml` file is below:

```YAML
version: "3.3"

services:
  ui1:
    container_name: wp
    image: httpd:2.4
    ports:
      - 10001:8080
    volumes:
      - ./volume:/home
    networks:
      demo-net:
        ipv4_address: 192.168.2.2

  ui2:
    container_name: wp2
    image: httpd:2.4
    ports:
      - 10002:8080
    volumes:
      - ./volume:/home
    networks:
      demo-net:
        ipv4_address: 192.168.2.3

  ui3:
    container_name: wp3
    image: httpd:2.4
    ports:
      - 10003:8080
    volumes:
      - ./volume:/home
    networks:
      demo-net:
        ipv4_address: 192.168.2.4

  ui4:
    container_name: wp4
    image: httpd:2.4
    ports:
      - 10004:8080
    volumes:
      - ./volume:/home
    networks:
      demo-net:
        ipv4_address: 192.168.2.5

  db:
    container_name: db
    image: mariadb:10.5.1
    restart: always
    environment:
      MYSQL_DATABASE: demodb
      MYSQL_USER: demouser
      MYSQL_PASSWORD: demopass
      MYSQL_RANDOM_ROOT_PASSWORD: "1"
    volumes:
      - db:/var/lib/mysql
    networks:
      demo-net:
        ipv4_address: 192.168.2.6
networks:
  demo-net:
    ipam:
      driver: default
      config:
        - subnet: "192.168.2.0/24"
volumes:
  ui:
  db:
```

---

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.  
