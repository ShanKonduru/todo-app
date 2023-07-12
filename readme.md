# Create multi-container apps with MySQL and Docker Compose

Tutorial: Create a Docker app with Visual Studio Code
https://learn.microsoft.com/en-us/visualstudio/docker/tutorials/docker-tutorial

Tutorial: Share a Docker app with Visual Studio Code
https://learn.microsoft.com/en-us/visualstudio/docker/tutorials/docker-tutorial-share

Tutorial: Persist data in a container app using volumes in VS Code
https://learn.microsoft.com/en-us/visualstudio/docker/tutorials/tutorial-persist-data-layer-docker-app-with-vscode


Tutorial: Create multi-container apps with MySQL and Docker Compose
https://learn.microsoft.com/en-us/visualstudio/docker/tutorials/tutorial-multi-container-app-mysql

## Highlevel steps 
- Start MySQL
- Run your app with MySQL
- Create a Docker Compose file
- Run the application stack
- Clean up resources
- Next steps

## Prerequisites

### Check the Docker Compose is installed, use the following command 

docker-compose version

### Start MySQL

### Step 0. Create the network by using this command.

docker network create todo-app

### Step 1. Start a MySQL container and attach it the network.

docker run -d --network todo-app --network-alias mysql -v todo-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=todos mysql:5.7

### Step 2. Run the following command, to view all containers available in the system.

docker ps
output:
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                 NAMES
848375361c93   mysql:5.7   "docker-entrypoint.s…"   13 minutes ago   Up 13 minutes   3306/tcp, 33060/tcp   nifty_gates

### Step 3. copy the container ID

### Step 4. Connect to Mysql DB 

docker exec -it 848375361c93 mysql -p
Enter the password as 'root', when prompted.

### Step 5. In the MySQL shell, list the databases and verify you see the todos database.

SHOW DATABASES;

Output
+-------------------------------+
| Database                        |
+-------------------------------+
| information_schema   |
| mysql                              |
| performance_schema |
| sys                                   |
| todos                              |
+-------------------------------+
5 rows in set (0.00 sec)

### Step 6. Enter exit when you're ready to return to the terminal command prompt.

### Step 7. The following command starts the app and connects that container to your MySQL container.

docker run -dp 3000:3000 -w /app -v "%cd%":/app --network todo-app -e MYSQL_HOST=mysql -e MYSQL_USER=root  -e MYSQL_PASSWORD=root -e MYSQL_DB=todos node:20-alpine sh -c "yarn install && yarn run dev"
