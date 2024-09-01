# Lab Manual: Working with Docker Containers to create a 3-tier application

**Objective:** In this lab, you will learn how to set up and manage Docker containers on a Linux host, including creating a 3-tier web application stack using separate containers for the frontend, backend, and database.

**Prerequisites:**

- Familiarity with basic command line operations (e.g., `bash`, `cd`, `mkdir`, `vim`)
- Basic knowledge of containerization concepts
- Linux server with Docker installed

**Step 1: Login to Play with Docker**

1. Open the web browser and go to [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/).
2. Create your docker account and then click on start to start your session.
3. When the session has started, click on `Add New Instance` to create a host to run docker containers.

**Step 2: Creating a New Folder for Your Project**

1. Create a new project folder called `project`.
2. Navigate to this folder
```bash
mkdir project
cd project
```

**Step 3: Building a Docker Image for the Frontend (Nginx)**

1. Create a new folder called `frontend` in the project folder
2. Create the html file `index.html`
```bash
mkdir frontend
cd frontend
touch index.html
```

3. Add the following contents to the `index.html` file:
```html
<!DOCTYPE html>  
<html>  
<body>  
  
<h1>Hello, I am {myname}</h1>  
<p>This is my first container project</p>  
  
</body>  
</html>
```
Note: replace `{myname}` in the above file with your real name.

4. Create a new file called `Dockerfile`.
5. Add the following contents to the `Dockerfile`:
```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

This Dockerfile uses the official Nginx image, copies an `index.html` file from your local machine into the container, exposes port 80, and sets a command to run when the container starts.

6. Save the file as `Dockerfile` (without any extension).

**Step 4: Running the Frontend Container**

1. Run the following command to build the frontend container image
```bash
docker build . -t my-frontend
```

2. Run the following command to list the images in local
```bash
docker image ls
```

3. Run the following command to run the frontend container
```bash
docker run -d --name frontend-container -p 80:80 my-frontend
```

4. Run the following command to check if your container is running
```bash
docker ps
```

**Step 5: Building a Docker Image for the Backend (Node.js)**

1. Create a folder called `backend` inside your project folder.
2. Create a file called `server.js` inside the `backend` folder.
```bash
cd ~/project
mkdir backend
cd backend
touch server.js
```
3. Add the following content to this file.
```Javascript
const http = require('http');

// Create a server object
http.createServer(function (req, res) {

	// http header
	res.writeHead(200, { 'Content-Type': 'text/html' });

	const url = req.url;

	if (url === '/about') {
		res.write(' Welcome to about us page');
		res.end();
	}
	else if (url === '/contact') {
		res.write(' Welcome to contact us page');
		res.end();
	}
	else {
		res.write('Hello World!');
		res.end();
	}
}).listen(3000, function () {
	// The server object listens on port 3000
	console.log("server start at port 3000");
});
```
4. Create a new file called `Dockerfile` inside the `backend` folder.
5. Add the following contents to the `Dockerfile`:

```dockerfile
FROM node:latest
WORKDIR /app
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

This Dockerfile uses the official Node.js image, sets up a workspace, copies your code into the container, exposes port 3000, and sets a command to run when the container starts.

6. Save the file as `Dockerfile` (without any extension).

**Step 6: Running the Backend Container**

1. Run the following command to build the backend container image
```bash
docker build . -t my-backend
```

2. Run the following command to list the images in local
```bash
docker image ls
```

3. Run the following command to run the backend container
```bash
docker run -d --name backend-container -p 3000:3000 my-backend
```

4. Run the following command to check if your container is running
```bash
docker ps
```

**Step 7: Building a Docker Image for the Database (PostgreSQL)**

1. Create a folder called `database` inside your project folder.
2. Create a file called `Dockerfile` inside the `database` folder.
```bash
cd ~/project
mkdir database
cd database
touch Dockerfile
```

3. Add the following contents to the `Dockerfile`:
```dockerfile
FROM mysql:latest
ENV MYSQL_ROOT_PASSWORD=mypassword
ENV MYSQL_DATABASE=mydb
```

This Dockerfile uses the official MySQL image, sets environment variables for a new user and database, and sets a command to run when the container starts.

4. Save the file as `Dockerfile` (without any extension).

**Step 8: Running the Database Container**

1. Run the following command to build the backend container image
```bash
docker build . -t my-database
```

2. Run the following command to list the images in local
```bash
docker image ls
```

3. Run the following command to run the backend container
```bash
docker run -d --name database-container -p 3306:3306 my-database
```

4. Run the following command to check if your container is running
```bash
docker ps
```

**Step 9: Verify Your Application**

1. Run `docker ps` command to see the 3 containers running
2. Use the `open port` option and try port 80 and 3000 to see the frontend and backend applications loading
3. Try stopping and starting the container using `docker stop <container-id>` and `docker start <container-id>` commands

Congratulations! You have successfully set up a 3-tier web application stack using Docker containers.
