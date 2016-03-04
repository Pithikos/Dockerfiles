Running Rails and Postgres in the same container
================================================

This Dockerfile builds an image running Rails and Ppostgres in the same
container. On top of that a database user is crated and some rake commands
are set to start every time you run a new container.


Build image
-----------
First add the Dockerfile to the root of your project. Once that is done
you can build the image.

    docker build --tag="myproject" .

By default postgres user `test` will be created and the rake commands
 `rake db:create && rake db:migrate` will be run every time you run a container
 from this image. To change this things have a look at the Dockerfile.


Run container
-------------
Once you have built an image for your project, you can run an instance
for it. Make sure you are in the root of your project.

    docker run -v $PWD:/src -p 4500:3000 myproject

Assuming you have a proper config/database.yml in your project set to
use postgres, you should be able and visit http://127.0.0.1:4500 and
see your project running!
