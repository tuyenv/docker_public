# Docker: Ubuntu, Nginx and PHP 7.0, Memcached
This is based on phusion/baseimage-docker base Ubuntu image

After cloning the project:

```sh
cd /path/to/repo
docker build -t webapp . # Build a Docker image named "webapp" from this location "."
# wait for it to build...

# Run the docker container
docker run -v /path/to/local/web/files:/var/www:rw -p 80:80 -d webapp /sbin/my_init --enable-insecure-key
```

This will bind local port 80 to the container's port 80. This means you should be able to go to "localhost" in your browser (or the IP address of your virtual machine oh which Docker is running) and see your web application files.
- docker run - starts a new docker container
- -v /path/to/local/web/files:/var/www:rw - Bind a local directory to a directory in the container for file sharing. rw makes it "read-write", so the container can write to the directory.
- -p 80:80 - Binds the local port 80 to the container's port 80, so local web requests are handled by the docker.
- -d webapp - Use the image tagged "webapp"
- /sbin/my_init - Run the init scripts used to kick off long-running processes and other bootstrapping, as per phusion/baseimage-docker
- --enable-insecure-key - Enable a generated SSL key so you can SSH into the container, again as per phusion/baseimage-docker. Generate your own SSH key for production use.
- If you use this with fideloper/docker-mysql, then link this container with MySQL's (after running the MySQL container first) via -link mysql:db
- You can mount a local directory to nginx log directory at /var/log/nginx/. Such as: -v /path/to/local/log/dir:/var/log/nginx/

Credits: https://github.com/fideloper/docker-nginx-php
