Strider CD with Docker support
==================================================
           
An image with StriderCD and Docker, all running
inside the same container.
           
The Dockerfile will always build the latest Jenkins,
Git, Docker and their plugins for the specified base
image (Ubuntu 14.04 by default).


Run 
==================================================

Make sure you are in the same folder as the Dockerfile and run
the below:

  1. `./preprocess`
  2. `docker build -t "strider" .`
  3. `docker run -d --privileged -v $PWD/data/db:/data/db -p 3000:3000 strider`
  4. Visit http://localhost:3000

preprocess is a script that will assure that the Docker installed inside
the container is the same version as the one outside of it.

All contect like user accounts, projects, etc. will be stored in `data/db`
