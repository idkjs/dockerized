# Dockerize and run a node.js app

## Steps

1. Create the node.js app w/ npm init
2. Create the server.
3. Create the Dockerfile
4. Dockerfile: add fields and commands for FROM, WORKDIR, COPY, RUN, Expose, and CMD
6. Add .dockerfileignore
7. Build the image.
8. Verify the image exists
9. Create a container and run the image.
10. Get output from container
11. Test the container.

## Notes

- npm init
- set main entry point, server.js
- set start script, "node server.js"
- create server.js and an express server
- install deps, express
- create Dockerfile, .dockerignore
- build the image `docker build -t idkjs/dockerized-app .` // note the period for cwd.
- check the new image is listed `docker images`
- run the image `docker run -p 49160:8080 -d idkjs/dockerized-app` - this did not work. The command that worked is `docker run -p 8080:8080 -d idkjs/dockerized-app`. See the duly upvoted <https://stackoverflow.com/questions/36813690/connection-refused-on-docker-container>

- If you read the explanation carefully, 49160:8080, is mapping 49160 in your browser to port 8080 in the container. So when the example says, running on 8080 it means, inside the container. If you hit http://localhost:49160 you get the intended result. My 8080:8080 just duplicated the first command except by going to the browser at 8080 was hitting the port the docker container expected to recieve its http request from. Of course, it said all this on the very next line. Remember that these people are smarter than you and if you read the documentation carefully, the answer should be there. The next line is `curl -i localhost:49160`.

- NOTE: Got caught here becaus CMD array was not spaced properly. Apparently the file is spacing sensitive. [ "npm", "start"] = bad, ["npm", "start"] = good. I deleted the image and container and recreated them.

- print output from running app and copy the container id. Run `docker ps` to get the container id

- docker ps output:

```bsh
CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS              PORTS                     NAMES
3d4106567b49        idkjs/dockerized-app   "npm start"         5 seconds ago       Up 11 seconds       0.0.0.0:49160->8080/tcp   eloquent_bhabha
```

- Take id from previous command, run `docker logs 3d4106567b49`
- Output:

```bsh
npm info it worked if it ends with ok
npm info using npm@3.10.10
npm info using node@v6.11.3
npm info lifecycle dockerized@1.0.0~prestart: dockerized@1.0.0
npm info lifecycle dockerized@1.0.0~start: dockerized@1.0.0

> dockerized@1.0.0 start /usr/src/app
> node server.js

Running on http://0.0.0.0:8080
```

## Bonus: Azure CLI in the container via https://twitter.com/pjausovec
Below is from <https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker>

Microsoft ships the latest Azure CLI as a Docker image. You can easily launch a container running the CLI from the Command Palette (press F1 and search for Docker: Azure CLI). The extension will then run an interactive terminal attached to the container.

After the container is started, you will be prompted to login to your Azure account. From there, set the subscription you want to work with using az account set (you can see all of your subscriptions with az account list). You do not need to login in every time you run the container becasue the extension volume mounts the local $HOME/.azure folder to the container's $HOME/.azure folder.

If you do not have an Azure subscription, sign up today for a free 30 day account and get $200 in Azure Credits to try out any combination of Azure services.


- In VS Code, open command pallete, start typing Docker Azure, hit enter.
 
## Resources

https://nodejs.org/en/docs/guides/nodejs-docker-webapp/
https://stackoverflow.com/questions/36813690/connection-refused-on-docker-container