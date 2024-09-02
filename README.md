## Lab Manual: Working with Docker Containers to run a web server

**Objective:** In this lab, you will learn how to set up and manage Docker containers on a Linux host, including creating a web server for hosting a html web page.

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

**Step 3: Building a Docker Image for the web server (Nginx)**

1. Create the html file `index.html`
```bash
touch index.html
```

2. Add the following contents to the `index.html` file:
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

3. Create a new file called `Dockerfile`.
4. Add the following contents to the `Dockerfile`:
```dockerfile
FROM public.ecr.aws/nginx/nginx:1.26
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

5. Use the `open port` option and try port 80 to see the web page loading
6. Try stopping the container using `docker stop frontend-container` commands

Congratulations! You have successfully set up a html web page over Nginx web server using Docker containers.
