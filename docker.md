
# Docker Installation and Usage Guide
## Install Docker on Arch Linux
### Update the package database
```bash
sudo pacman -Syu
```
### Install Docker
```bash
sudo pacman -S docker
```
### Start and enable the Docker service
```bash
sudo systemctl start docker
sudo systemctl enable docker
```
### Add your user to the docker group
to run Docker commands without `sudo`, add your user to the `docker` group:
```bash
sudo usermod -aG docker $USER
```
Log out and log back in for the group changes to take effect



## Sign in to Docker Hub
You can sign in to Docker Hub using the Docker CLI:
```bash
docker login
```
If available this command will take you to web login page. Otherwise, it will prompt you for your Docker Hub username and password. After entering your credentials, you should be successfully logged in. 

### Verify Login
To verify that you are logged in, you can run:
```bash
docker info
```
This command should display your Docker Hub username if you are logged in. The docker login command is the standard way to authenticate with Docker Hub on Linux systems. If you encounter issues, consider using a Personal Access Token for authentication.



## How to Use Docker
Since Docker Desktop is not offically available on Arch Linux, I recommend using Docker Engine and the Docker CLI to manage your Docker containers and images. But if you are new to docker and linux enviorement you should start with docker desktop. Once you know your way you can always use CLI.

### Using Docker CLI
Once logged in, you can push images to Docker Hub:
```bash
docker tag <image-id> <your-docker-hub-username>/<repository-name>:<tag>
docker push <your-docker-hub-username>/<repository-name>:<tag>
```
### Install Docker Desktop
install docker-desktop with yay
```bash
yay -S docker-desktop
```
this will ask you to isntall qemu vm you can safely choose the base installlation. Usually its the first choice.

To start Docker Desktop for Linux:
```bash
systemctl --user start docker-desktop #to start docker desktop
systemctl --user stop docker-desktop #to stop docker desktop
```
Alternatively, open a terminal and run:
```bash
docker desktop start
docker desktop stop
```
You might see that docker is not likeo other applications when you close it it doesnot closes if you want to open just the gui. Try
```bash
docker desktop restart
```
To enable Docker Desktop to start on sign in:
```bash
systemctl --user enable docker-desktop
```