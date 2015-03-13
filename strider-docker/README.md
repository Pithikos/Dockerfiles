Strider CD with Docker support
==================================================

An image with StriderCD and Docker, all running
inside the same container.
           
The latest Strider will be built and the `preprocess` script
in this file will make sure that the appropriate docker
container is installed. (Notice if you update your machine's
Docker you will need to rebuild the image.)


Run
==================================================

Make sure you are in the same folder as the Dockerfile and run
the below:

  1. `./preprocess`
  2. `docker build -t strider .`
  3. `docker run -d --privileged -v $PWD/data/db:/data/db -p 3000:3000 strider`
  4. Go to [http://localhost:3000](http://localhost:3000)

preprocess is a script that will assure that the Docker installed inside
the container is the same version as the one outside of it.

All content like user accounts, projects, etc. will be stored in `data/db`.


Add an account
==================================================

By default the container is made with some security in mind. For that reason
there is not even a default user. To add a user you can either use the
[docker-enter tool](https://github.com/Pithikos/docker-enter) or `docker exec`.

Once in the container simply run:

  1. strider addUser
  
Create the user following the instructions. Once done, you should be able
and login at [http://localhost:3000](http://localhost:3000) without
restarting anything.
