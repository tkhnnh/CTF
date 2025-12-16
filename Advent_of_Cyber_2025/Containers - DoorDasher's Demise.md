# What is a container
Containers share the host OS kernel, isolating only applications and their dependencies, which makes them lightweight and fast to start. 
=> Virtual machines are ideal for running multiple different operating systems or legacy applications, while containers excel at deploying scalable, portable micro-services.

# Escape Attack & Sockets
A container escape is a technique that enables code running inside a container to obtain rights or execute on the host kernel (or other containers) beyond its isolated environment (escaping).

# Challenge
Attempting to run code inside containers
Based on documentation, setting called "Enhanced Container Isolation" blocking containers from mounting the Docker socket to prevent malicious access to Docker Engine

In some case, they need Docker socket access -> providing means access via API directly => use that to confirm that we can perform Docker commands and interact with API or perform Docker Escape attack

`deployer` happens to be privileged container -> `docker exec -it deployer bash` -> creating a bash by executing deployer

What exact command lists running Docker containers?
`docker ps`

What file is used to define the instructions for building a Docker image?
`Dockerfile`

What is the flag?
`THM{DOCKER_ESCAPE_SUCCESS}`

Bonus question:
`DeployMaster2025!`
