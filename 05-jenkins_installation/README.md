# Simplifying Jenkins Setup: Local and Remote Deployment with Docker and AWS

## Running Jenkins Locally with Docker

If you're venturing into continuous integration, Jenkins is a powerful tool to automate your workflows. Setting it up locally can be made easy with Docker. Let's break down the Docker run command for a smooth Jenkins experience.

### Docker Run Command:

```bash
docker run -u root --rm -d -p 8080:8080 -p 50000:50000 -v jenkins-vol:/var/jenkinshome -v /var/run/docker.sock:/var/run/docker.sock jenkins:jenkins:lts

```

**1. `docker run`**: Initiates a Docker container.

**2. `-u root`**: Runs the container as the root user for necessary permissions.

**3. `--rm`**: Automatically removes the container when it stops for a clean environment.

**4. `-d`**: Runs the container in the background for concurrent terminal use.

**5. `-p 8080:8080`**: Maps local port 8080 to the container for Jenkins web access.

**6. `-p 50000:50000`**: Maps port 50000 for Jenkins agent communication.

**7. `-v jenkins-vol:/var/jenkinshome`**: Creates a named volume "jenkins-vol" for persistent data storage.

**8. `-v /var/run/docker.sock:/var/run/docker.sock`**: Connects to the Docker daemon for container management.

**9. `jenkins:jenkins:lts`**: Specifies the Jenkins Long Term Support (LTS) version.

### Setup Achievements:

- **User Permissions**: Running as root ensures container permissions.
- **Automated Cleanup**: Automatic removal keeps the environment tidy.
- **Detached Mode**: Allows concurrent terminal use.
- **Port Mapping**: Maps ports for Jenkins web access and agent communication.
- **Volume Mounts**: Ensures data persistence across container restarts.
- **Docker Daemon Integration**: Connects Jenkins to the Docker daemon for container management.

### Setting Up Jenkins Locally:

1. Execute the Docker run command.
2. Access Jenkins at [http://localhost:8080](http://localhost:8080/).
3. Retrieve the initial admin password:
    
    ```bash
    docker exec -it <container_id> cat /var/jenkinshome/secrets/initialAdminPassword
    
    ```
    
4. Complete the Jenkins setup wizard.

You now have a local Jenkins environment ready for continuous integration.

## Simplifying Jenkins Setup with Docker Compose

Jenkins setup can get even simpler with Docker Compose. Follow these steps:

### 1. Directory Setup:

```bash
mkdir jenkins-docker
cd jenkins-docker

```

### 2. Docker Compose File (`docker-compose.yml`):

```yaml
version: "3.5"
services:
    jenkins:
        container_name: jenkins-docker
        image: jenkins/jenkins:lts
        ports:
            - 8080:8080
        networks:
            - devnetwork
networks:
    devnetwork:
        name: devnetwork

```

**Compose File Breakdown:**

- `container_name`: Sets the container name to "jenkins-docker."
- `image`: Specifies the Jenkins LTS version.
- `ports`: Maps host port 8080 to container port 8080.
- `networks`: Connects to the "devnetwork."

### 3. Docker Compose Commands:

```bash
docker network create -d bridge devnetwork
docker-compose up -d
docker-compose logs -f
docker ps
docker exec -it jenkins-docker bash
docker exec -it jenkins-docker /var/lib/jenkins_home/secrets/initialAdminPassword

```

**Commands Explanation:**

- `docker network create -d bridge devnetwork`: Creates a Docker network.
- `docker-compose up -d`: Builds and starts Jenkins in detached mode.
- `docker-compose logs -f`: Displays real-time logs.
- `docker ps`: Lists running Docker containers.
- `docker exec -it jenkins-docker bash`: Opens an interactive bash shell.
- `docker exec -it jenkins-docker /var/lib/jenkins_home/secrets/initialAdminPassword`: Retrieves the initial admin password.

**In Summary:**

Docker Compose simplifies Jenkins setup with a single configuration file and easy-to-use commands.

## Jenkins on AWS EC2: A Simple Guide

### Running Jenkins Remotely on an EC2 Instance

**Step 1: Launch EC2 Instance on AWS:**

1. Open AWS Management Console.
2. Launch an EC2 instance with Ubuntu AMI.
3. Configure details, storage, security groups, and launch.
4. Create/choose a key pair for SSH access.

**Step 2: Connect to EC2 Instance from Local Terminal:**

```bash
chmod 400 your-key.pem
ssh -i your-key.pem ubuntu@your-instance-ip

```

**Installing Jenkins on EC2 Ubuntu:**

Execute the following commands:

```bash
sudo -s
apt-get update
apt-get install -y openjdk-8-jdk
java -version
wget -q -O - <https://pkg.jenkins.io/debian/jenkins.io.key> | sudo apt-key add -
sh -c 'echo deb <http://pkg.jenkins.io/debian-stable> binary/ > /etc/apt/sources.list.d/jenkins.list'
apt-get update
apt-get install -y jenkins

```

**Checking and Configuring Jenkins on Ubuntu Server:**

1. Check Jenkins Service Status:

```bash
systemctl status jenkins

```

1. Check Jenkins Process:

```bash
ps -ef | grep jenkins

```

1. Check UFW (Uncomplicated Firewall) Status:

```bash
ufw status

```

1. Allow Traffic on Port 8080:

```bash
ufw allow 8080

```

**Retrieve Initial Admin Password for Jenkins:**

Use the `cat` command:

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword

```

By following these steps, Jenkins is set up on an EC2 instance. Access Jenkins through your browser, complete the setup wizard, and start automating your CI/CD pipelines.

**In Conclusion:**

Congratulations! You've set up Jenkins locally using Docker and on a remote host. These steps provide a foundation for creating CI/CD pipelines and automating software development workflows. Happy automating! Whether locally with Docker or remotely on AWS EC2, setting up Jenkins is made simpler. Choose the method that suits your needs and dive into the world of Jenkins for efficient automation in your development workflows. Happy automating!

