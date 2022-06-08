# docker_firefox_on_jammy
Demonstrates how to install and run Firefox (non-snap) inside an Ubuntu 22.04 LTS docker container

Recent Ubuntu distros don't allow `/usr/bin/firefox` to run unless it's installed with `snap`. Based on the instructions [here](https://www.omgubuntu.co.uk/2022/04/how-to-install-firefox-deb-apt-ubuntu-22-04#:~:text=Installing%20Firefox%20via%20Apt%20(Not%20Snap)&text=You%20add%20the%20Mozilla%20Team,%2C%20bookmarks%2C%20and%20other%20data.) and [here](https://gursimar27.medium.com/run-gui-applications-in-a-docker-container-ca625bad4638), this container configures `apt` in order to install Firefox from the Mozilla repository, and launches Firefox in a window on the host.

Note: I've modified some files on a Windows PC after testing on Ubuntu; this might've broken some things.

### Usage

1. Before launching the image, a file `xauth_list` must be created by running `xauth nlist > xauth_list` in the same folder as `docker-compose.yml`. This contains the authentication tokens required to connect to the X display.
2. Run with `docker compose up -d`. This will launch a Firefox window on the host.

### Possible improvements
* The Mozilla PPA repo is added with `apt-add-repository`, which is quite a large addition to the image with all its dependencies. This step could be replaced with a 2-tier Dockerfile that only copies the resulting files. Thanks to Firefox, the resulting image is large, anyway (~500 MB). For the purposes of demonstration, I did not bother with more than minimal optimisation.
* VNC instead of connecting to X on host
* etc
