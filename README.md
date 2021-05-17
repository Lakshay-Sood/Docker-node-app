# Docker Node App
A simple node server that we will run in docker container using its docker image.

## Prerequisite:
1. Get Docker from https://docs.docker.com/get-docker/
2. In the terminal, type `docker version` and it should list *Client* as well as *Server Docker Engine* details
(if only *Client* details are displayed, make sure your Docker desktop app is up and running)

## Steps to Follow:
1. Clone this repo
  1. Open a new directory on your local machine
  2. In the terminal, type `git clone https://github.com/Lakshay-Sood/Docker-node-app.git`
2. The Dockerfile and docker-compose.yml files are already set up
3. Run `docker compose up`
4. Voilā! You are Done. Your app should be available on http://localhost:8000/

## What's actually happening:
- ### **app.js**
  1. Creating a simple express (node) server
  2. On a GET request at '/', it displays an HTML page (`index.html`) and passes on a message variable to display it
  
- ### **index.html**
  1. Just displays the *message* that was passed by the `app.js` file
  2. the *meta* tag instructs it to refresh every 2 seconds and thus next *message* appears every 2 seconds on the localhost
  
- ### **package.json**
  1. Specifies the entry point to our program, i.e., `app.js`
  2. Lists the dependancies: express and express-handlebars
  
- ### **Dockerfile**
  1. Specifies the base docker image as *node:8.2.1-alpine* which is a node image based on alpine OS (very lightweight Linux distro)
  2. Specifies that work directory inside docker container would be */code* (its arbitary)
  3. Installs nodemon so that our server can re-run on code changes
  4. Moves `package.json` and `node_modules` into the work directory
  5. Does `npm install` to install modules listed in `package.json`
  6. Copies our source code from current directory (on local machine) to */code* directory in the docker's virtual environment
  7. Specifies a command `npm start` that should be run when we run the docker container
  
- ### **docker-compose.yml**
  1. Specifies a service *web* for our docker image of node server
  2. `build .` tells docker to build an image using `Dockerfile` in the current directory (*.* specifies current directory)
  3. `nodemon` command is for debugging purposes
  4. `volumes` specifies that our current directory (*.*) in local machine should be mapped to the */code* directory in the docker's virtual environment. 
  This is important as any change we make on our local directory is then reflected in docker and thus nodemon can track it and restart the server with updated code
  5. Maps the ports 8000 (and 5858, for debugging) of the local machine to the ports 8000 (and 5858) of the docker's virtual environment. 
  Thus, requests made to our localhost:8000 will be redirected to port 8000 of the docker container where our server is actually running.
  
