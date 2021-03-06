# What is DevOps?

Before we get started with Docker let’s lay the groundwork for learning the right mindset. Defining DevOps is not a trivial task but the term itself consists of two parts, Dev and Ops. Dev refers to the development of software and Ops to operations. Simple definition for DevOps would be that it means the release, configuring, and monitoring of software is in the hands of the very people who develop it.

A more formal definition is offered by Jabbari et al.: “DevOps is a development methodology aimed at bridging the gap between Development and Operations, emphasizing communication and collaboration, continuous integration, quality assurance and delivery with automated deployment utilizing a set of development practices”.

![DevOps](https://upload.wikimedia.org/wikipedia/commons/0/05/Devops-toolchain.svg)

Sometimes DevOps is regarded as a role that one person or a team can fill. Here’s some external motivation to learn DevOps skills: Salary by Developer Type in StackOverflow survey. You will not become a DevOps specialist solely from this course, but you will get the skills to help you navigate in the increasingly containerized world.

During this course we will focus mainly on the packaging, releasing and configuring of the applications. You will not be asked to plan or create new software. We will go over Docker and a few technologies that you may see in your daily life, these include e.g. Redis and Postgres. See StackOverflow survey on how closely they correlate these technologies.

## Exercises

### 1.1: Getting started
Since we already did “Hello, World!” in the material let’s do something else.
Start 3 containers from image that does not automatically exit, such as nginx, detached.
Stop 2 of the containers leaving 1 up.
Submit the output for `docker ps -a` which shows 2 stopped containers and one running.

### 1.2: Cleanup
We’ve left containers and a image that won’t be used anymore and are taking space, as `docker ps -as` and `docker images` will reveal.
Clean the docker daemon from all images and containers.
Submit the output for `docker ps -a` and `docker images`

### 1.3: Secret message
Now that we’ve warmed up it’s time to get inside a container while it’s running!
Image `devopsdockeruh/simple-web-service:ubuntu` will start a container that outputs logs into a file. Go inside the container and use `tail -f ./text.log` to follow the logs. Every 10 seconds the clock will send you a “secret message”.
Submit the secret message and command(s) given as your answer.

### 1.4: Missing dependencies
Start a ubuntu image with the process `sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'`

You will notice that a few things required for proper execution are missing. Be sure to remind yourself which flags to use so that the read actually waits for input.

> *Note also that curl is NOT installed in the container yet. You will have to install it from inside of the container.
Test inputting helsinki.fi into the application. It should respond with something like*

```<html>

<head>
  <title>301 Moved Permanently</title>
</head>

<body>
  <h1>Moved Permanently</h1>
  <p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
</body>

</html>
``` 
This time return the command you used to start process and the command(s) you used to fix the ensuing problems.

> *This exercise has multiple solutions, if the curl for helsinki.fi works then it’s done. Can you figure out other (smart) solutions?*

### 1.5: Sizes of images
In a previous exercise we used `devopsdockeruh/simple-web-service:ubuntu`.
Here is the same application but instead of ubuntu is using alpine: `devopsdockeruh/simple-web-service:alpine`.
Pull both images and compare the image sizes. Go inside the alpine container and make sure the secret message functionality is the same. Alpine version doesn’t have bash but it has sh.

### 1.6: Hello Docker Hub
Run `docker run -it devopsdockeruh/pull_exercise.`
It will wait for your input. Navigate through docker hub to find the docs and Dockerfile that was used to create the image.
Read the Dockerfile and/or docs to learn what input will get the application to answer a “secret message”.
Submit the secret message and command(s) given to get it as your answer.

### 1.7: Two line Dockerfile
By default our `devopsdockeruh/simple-web-service:alpine` doesn’t have a CMD. It instead uses *ENTRYPOINT* to declare which application is run.
We’ll talk more about *ENTRYPOINT* in the next section, but you already know that the last argument in `docker run` can be used to give command.
As you might’ve noticed it doesn’t start the web service even though the name is “simple-web-service”. A command is needed to start the server!
Try `docker run devopsdockeruh/simple-web-service:alpine hello`. The application reads the argument but will inform that hello isn’t accepted.
In this exercise create a Dockerfile and use FROM and CMD to create a brand new image that automatically runs the server. Tag the new image as “web-server”
Return the Dockerfile and the command you used to run the container.
Running the built “web-server” image should look like this:
```$ docker run web-server
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /*path                    --> server.Start.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```
> *We don’t have any method of accessing the web service yet. As such confirming that the console output is the same will suffice.*

### 1.8: Image for script
Now that we know how to create and build Dockerfiles we can improve previous works.
Create a Dockerfile for a new image that starts from ubuntu:18.04.
Make a script file on you local machine with such content as `echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;`. Transfer this file to an image and run it inside the container using CMD. Build the image with tag “curler”.
Run the new `curler` image with the correct flags and input helsinki.fi into it. Output should match the 1.4 one.
Return both Dockerfile and the command you used to run the container.

### 1.9: Volumes
In this exercise we won’t create a new Dockerfile.
Image `devopsdockeruh/simple-web-service` creates a timestamp every two seconds to `/usr/src/app/text.log` when it’s not given a command. Start the container with bind mount so that the logs are created into your filesystem.
Submit the command you used to complete the exercise.

### 1.10: Ports open
In this exercise we won’t create a new Dockerfile.
Image `devopsdockeruh/simple-web-service` will start a web service in port `8080` when given the command “server”. From 1.7 you should have an image ready for this. Use -p flag to access the contents with your browser. The output to your browser should be something like: `{ message: "You connected to the following path: ...`
Submit your used commands for this exercise.

### 1.11: Spring
Lets create a Dockerfile for a Java Spring project: [github page](https://github.com/docker-hy/material-applications/tree/main/spring-example-project)
The setup should be straightforward with the README instructions. Tips to get you started:
Use [openjdk image](https://hub.docker.com/_/openjdk) `FROM openjdk:_tag_`to get java instead of installing it manually. Pick the tag by using the README and dockerhub page.
You’ve completed the exercise when you see a ‘Success’ message in your browser.

### 1.12: Hello, frontend!
**This exercise is mandatory**
A good developer creates well written READMEs that can be used to create Dockerfiles with ease.
Clone, fork or download the project from https://github.com/docker-hy/material-applications/tree/main/example-frontend.
Create a Dockerfile for the project (example-frontend) and give a command so that the project runs in a docker container with port 5000 exposed and published so when you start the container and navigate to http://localhost:5000 you will see message if you’re successful.
Submit the Dockerfile.
As in other exercises, do not alter the code of the project
> TIP: The project has install instructions in README.
> TIP: Note that the app starts to accept connections when “Accepting connections at http://localhost:5000” has been printed to the screen, this takes a few seconds
> TIP: You do not have to install anything new outside containers.

### 1.14: Environment
***This exercise is mandatory***
Start both frontend-example and backend-example with correct ports exposed and add ENV to Dockerfile with necessary information from both READMEs ([front](https://github.com/docker-hy/material-applications/tree/main/example-frontend),[back](https://github.com/docker-hy/material-applications/tree/main/example-backend)).
Ignore the backend configurations until frontend sends requests to `_backend_url_/ping` when you press the button.
You know that the configuration is ready when the button for 1.14 of frontend-example responds and turns green.
Do not alter the code of either project
Submit the edited Dockerfiles and commands used to run.

![](https://docker-hy.github.io/images/exercises/back-and-front.png)

> The frontend will first talk to your browser. Then the code will be executed from your browser and that will send a message to backend.

![](https://docker-hy.github.io/images/exercises/about-connection-front-back.png)

> TIP: When configuring web applications keep browser developer console ALWAYS open, F12 or cmd+shift+I when the browser window is open. Information about configuring cross origin requests is in README of the backend project.
> TIP: Developer console has multiple views, most important ones are Console and Network. Exploring the Network tab can give you a lot of information on where messages are being sent and what is received as response!

### 1.15: Homework
Create Dockerfile for an application or any other dockerised project in any of your own repositories and publish it to Docker Hub. This can be any project except clones / forks of backend-example or frontend-example.
For this exercise to be complete you have to provide the link to the project in docker hub, make sure you at least have a basic description and instructions for how to run the application in a [README](https://help.github.com/en/articles/about-readmes) that’s available through your submission.

### 1.16: Heroku
Pushing to heroku happens in a similar way. A project has already been prepared at `devopsdockeruh/heroku-example` so lets pull that first. Note that the image of the project is quite large.
Go to https://www.heroku.com/ and create a new app there and install heroku CLI. You can find additional instructions from `Deploy` tab under `Container Registry`. Tag the pulled image as `registry.heroku.com/_app_/_process-type_`, process-type can be `web` for this exercise. The app should be your project name in heroku.
Then push the image to heroku with `docker push registry.heroku.com/_app_/web` and release it using the heroku CLI: `heroku container:release web` (you might need to login first: `heroku container:login`)
For this exercise return the url in which the released application is.
You could also use the heroku CLI to build and push, but since we didn’t want to build anything this time it was easier to just tag the image.