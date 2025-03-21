# MATLAB Installation with Docker
## Pull the MATLAB Container Image
To download the MATLAB container image, use the following command. You must replace `r20XYz` with the specific MATLAB release (e.g., `r2022a`). Downloading and extracting the container image can take some time.
```bash
docker pull mathworks/matlab:r20XYz
```
### Run Container in Browser Mode
Run the MATLAB container using this command:
```bash
docker run -it --rm -p 8888:8888 --shm-size=512M mathworks/matlab:r20XYz -browser
```
1. `-it` Runs the container in interactive mode.
2. `--rm` Deletes the container when it is closed.
3. `-p 8888:8888` Exposes port 8888 for the web browser connection.
4. `--shm-size=512M`  Sets the shared memory size to 512 MB (required for MATLAB).
5. `-browser` Enables MATLAB to be accessed via a web browser.
After running the command, open a web browser and navigate to http://localhost:8888 to access MATLAB.

### Run Container in VNC Mode
You can also run the MATLAB container in VNC mode. Doing so allows you to install toolboxes or update MATLAB.
Run the MATLAB container in VNC mode by entering this command:
```bash
docker run --init -it --rm -p 5901:5901 -p 6080:6080 --shm-size=512M mathworks/matlab:r20XYz -vnc
```
1. `-p 5901:5901` Exposes port 5901 for the VNC connection.
2. `-p 6080:6080` Exposes port 6080 for the web browser connection.
3. `-vnc` Enables MATLAB to be accessed via a VNC client.

To connect to the desktop where you can open MATLAB:
1. Using a VNC Client: Connect to `localhost:1` (or the appropriate display number).
2. Using a Web Browser: Navigate to `http://localhost:6080` if you don’t have a VNC client installed.

If your container is not running on your local machine, replace localhost with the fully qualified domain name (FQDN) of the computer on which the container is running.

The default VNC password is `matlab`.

For a full list of options and environment variables that you can use to start the container, run the container with the -help flag:
```bash
docker run -it --rm mathworks/matlab:r20XYz -help
```