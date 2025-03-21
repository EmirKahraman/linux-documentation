
# Install Docker
Install the package:
```bash
# Update the package database
sudo pacman -Syu

# Install Docker
sudo pacman -S docker

# Start and enable the Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Add your user to the docker group to run Docker commands without sudo
sudo usermod -aG docker $USER

# Log out and log back in for the group changes to take effect
```
2. Sign in to Docker Hub via CLI

You can sign in to Docker Hub using the Docker CLI:
bash
Copy

docker login

This command will prompt you for your Docker Hub username and password. After entering your credentials, you should be successfully logged in.
3. Troubleshooting

If you encounter issues with the docker login command, ensure that:

    Docker is running (sudo systemctl status docker).

    You have an active internet connection.

    Your Docker Hub credentials are correct.

4. Alternative: Use a Personal Access Token

If you prefer not to use your Docker Hub password, you can generate a Personal Access Token (PAT) and use that for authentication:

    Go to Docker Hub and log in.

    Navigate to Account Settings > Security > New Access Token.

    Generate a token with the appropriate permissions.

    Use the token as your password when running docker login.

bash
Copy

docker login -u <your-docker-hub-username>

When prompted for the password, enter the Personal Access Token.
5. Verify Login

To verify that you are logged in, you can run:
bash
Copy

docker info

This command should display your Docker Hub username if you are logged in.
6. Push Images

Once logged in, you can push images to Docker Hub:
bash
Copy

docker tag <image-id> <your-docker-hub-username>/<repository-name>:<tag>
docker push <your-docker-hub-username>/<repository-name>:<tag>

Summary

Since Docker Desktop is not available on Arch Linux, you should use Docker Engine and the Docker CLI to manage your Docker containers and images. The docker login command is the standard way to authenticate with Docker Hub on Linux systems. If you encounter issues, consider using a Personal Access Token for authentication.

# Install Docker Desktop
To install docker desktop ypu have two ways.
### manual install
Install the Docker client binary on Linux. Static binaries for the Docker client are available for Linux as docker. You can use:
```bash
wget https://download.docker.com/linux/static/stable/x86_64/docker-28.0.2.tgz -qO- | tar xvfz - docker/docker --strip-components=1
mv ./docker /usr/local/bin
```

Download the latest Arch package from the Release notes(https://docs.docker.com/desktop/release-notes/).

Install the package:
```bash
sudo pacman -U ./docker-desktop-x86_64.pkg.tar.zst
```
By default, Docker Desktop is installed at /opt/docker-desktop.

### use yay
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

# Signing in with Docker Desktop for Linux

Docker Desktop for Linux relies on pass

to store credentials in gpg2-encrypted files. Before signing in to Docker Desktop with your Docker ID, you must initialize pass. Docker Desktop displays a warning if you've not initialized pass.

You can initialize pass by using a gpg key. To generate a gpg key, run:
```bash
gpg --generate-key
```
The following is an example similar to what you see once you run the previous command:

...
GnuPG needs to construct a user ID to identify your key.
```bash
Real name: Molly
Email address: molly@example.com
You selected this USER-ID:
   "Molly <molly@example.com>"

Change (N)ame, (E)mail, or (O)kay/(Q)uit? O
...
pubrsa3072 2022-03-31 [SC] [expires: 2024-03-30]
 <generated gpg-id public key>
uid          Molly <molly@example.com>
subrsa3072  2022-03-31 [E] [expires: 2024-03-30]
```
To initialize pass, run the following command using the public key generated from the previous command:
```bash
pass init <your_generated_gpg-id_public_key>
```
The following is an example similar to what you see once you run the previous command:
```bash
mkdir: created directory '/home/molly/.password-store/'
Password store initialized for <generated_gpg-id_public_key>
```
Once you initialize pass, you can sign in and pull your private images. When Docker CLI or Docker Desktop use credentials, a user prompt may pop up for the password you set during the gpg key generation.
```bash
docker pull molly/privateimage
Using default tag: latest
latest: Pulling from molly/privateimage
3b9cc81c3203: Pull complete 
Digest: sha256:3c6b73ce467f04d4897d7a7439782721fd28ec9bf62ea2ad9e81a5fb7fb3ff96
Status: Downloaded newer image for molly/privateimage:latest
docker.io/molly/privateimage:latest
```

If everything fails try
```bash
docker login
```
2
### Take a look at
(https://docs.docker.com/desktop/setup/install/linux/archlinux/)