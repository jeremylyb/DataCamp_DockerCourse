Command lines

1. Run local Image

   docker run <image-name>

2. Run Docker Hub container

   docker run ubuntu

3. Run Docker interactively with -it flag, this will start a container + interactive shell of the image/process/software in the container

   docker run -it <image-name>
   exit - to exit interactive shell thereafter

4. Run Docker detached mode with -d flag. This will start a container running in the background and not occupy your terminal session

   docker run -d <image-name>

5. See all container details

   docker ps

6. Stop container

   docker <container-id>

7. Naming a container

   docker run --name <container-name> <image-name>

8. Stop specific named container

   docker stop <container-name>
   docker stop <container-id>

9. Filter for specific container in get container details

   docker ps -f "name=<container-name>"

10. View docker output logs

    docker logs <container-id>

11. View docker output logs real time

    docker logs -f <container-id>
    ctrl c to exit live log

12. Delete container. Stop container does not delete it entirely.

    docker container rm <container-id>

13. Pull Image from Docker Hub repo

    docker pull <image-name>

14. Pull specific version

    docker pull <image-name>:<image-version>

15. View all images

    docker images

16. Remove Image

    docker image rm <image-name>

17. Remove all unused container in image

    docker image prune -a

18. Pull image from private registry

    docker pull <private-registry-url>/<image-name>:<image-version>

19. Push image to registry

    docker image push <image-name>

20. Push image to specific registry

    step 1. Rename:

    docker tag <old-name><new-name>

    step 2. push

    docker image push <private-registry-url>/<new-name>

21. Login to secured private docker registries

    docker login <private-registry-url>

22. Send docker image as files. Without path specified, it will save in current working directory

    docker save -o <file-name> <image-name>

23. Load image from file

    docker load -i <file-name>

24. Customizing Image through Dockerfile. This is different from "run docker <image-name>" directly, which create a container directly from the specified image

    - Step 1. Create Dockerfile

    touch Dockerfile/nano Dockerfile
    echo "COPY ./pipeline_v3/ /app" >> Dockerfile

    - Step 2. create commands within the Dockerfile

    FROM <image-name>:<image-version>

    - Step 3. Add other commands in

    - Step 4. build Dockerfile at current working directory (".") or specified path

    docker build .
    docker build -t <image-name>:<version> .

    - Step 5. run the built image

    run docker <image-name>

25. Add other instructions into the Dockerfile

    - Add Package manager

    FROM ubuntu
    RUN apt-get update
    RUN ap-get install -y python3

    - Copy files from host machine to image

    COPY <file-source-path> <image-destination-file-path>

    - Copy whole folder from host machine to image

    COPY <folder-source-path> <image-destination-folder-path>

    - Download files from sites with chaining commands

    RUN curl <file_download_url> -o <destination_directory>/<filename>.zip \
    && unzip <destionation_directory>/<filename>.zip -d <unzipped-directory> \
    && rm <destination_directory>/<filename>.zip

26. Add Command line instructions into Dockerfile

    - Initiate different software session through CMD command in Dockerfile so that upon initiating a build of container from the image, it will execute

    CMD python3/ CMD postgres
    FROM ubuntu
    ...

    NOTE: you can only have 1 CMD. But you can use CMD to run a script and have that script starts multiple different applicaitons (Nested CMD)

27. Override default start commnad of an image

    docker run <image-name> <shell-command>

28. Starting an image interactively with a custom start command with -it flag

    docker run -it <image-name> <shell-command>

29. Change the working directory of subsequent instructions within the Dockerfile

    Orig:
    COPY /projects/pipeline_v3/ /home/my_user_with_a_long_name/work/projects/app/

    With WORKDIR:
    WORKDIR /home/my_user_with_a_long_name/work/projects/
    COPY /projects/pipeline_v3/ app/

    NOTE: Overriding command will also be run in WORKDIR

30. Change USER permission in the Dockerfile for the container.

    NOTE: Typically, Images from Docker hub repo are Root users by default. Hence, need to change to limit access to the user

    FROM <Image-name> --> root user by default
    USER repl --> changes the user to repl
    RUN apt-get update --> Run as repl

31. Creating ARG variables in Dockerfile

    FROM <image-name>
    ARG <var_name>=<var_value>
    RUN apt-get install python3=$<ver_name>

32. Insert build-time variables in to Docker build process

    docker build --build-arg <var_name>=<var_value>

    NOTE: ARG variable is only accessible during the build process.

33. Creating ENV variables in Dockerfile for build-time or in run-time

    - In Dockerfile for build-time
      FROM <image-name>
      ENV APP_HOME=/app
      WORKDIR $APP_HOME

    - At run-time with -e flag
      docker run --env <key>=<value> <image-name>
      docker run -e ENV_VAR=value <image-namge>
