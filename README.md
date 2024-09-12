# Kettlewright Setup

1. Create a directory for environment variables and the SQLite database:
   
       mkdir -p ~/docker/kettlewright/instance

2. Copy `.env.template` to the directory and rename it to `.env`:
   
       cp [repo_directory]/.env.template ~/docker/kettlewright/.env

3. Edit `.env` with appropriate values.

4. Pull the Docker image:
   
       docker pull yochaigal/kettlewright

5. Run the container with environment file and bind the local `instance` folder:
   
       docker run -it --user $(id -u):$(id -g) --env-file .env -v $(pwd)/instance:/app/instance yochaigal/kettlewright /bin/sh

6. Inside the container, run database migrations:
   
       flask db upgrade

7. Exit the container:
   
       exit

8. Start the Flask application:
   
       docker run --user $(id -u):$(id -g) --env-file .env -v $(pwd)/instance:/app/instance -p 8000:8000 --restart always yochaigal/kettlewright

9. Open http://127.0.0.1:8000 to access Kettlewright. You can stop the application with `Ctrl+C` in the terminal.

## Docker Tips

To run the application again, first find the container id (typically the most recent container):

       docker ps -a

Then start the container with:

       docker start [container]

To see the logs, run:

       docker logs -f [container]

To remove old containers:

       docker rm [container] 

First, stop the container:

       docker stop [container]

Then remove it:

       docker rm [container]

Pull the latest image:

       docker pull yochaigal/kettlewright

Start a new container using the latest image:

       docker run --user $(id -u):$(id -g) --env-file .env -v $(pwd)/instance:/app/instance -p 8000:8000 --restart always yochaigal/kettlewright