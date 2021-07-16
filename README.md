# Create an DEB with a systemd service via CPack
This project uses CPack to create an DEB containing a systemd
service. I'm a novice when in comes to creating DEBs

Here's a utilitarian user's guide:
```bash
# Clone, build, and package the project.
mkdir build
cd build
cmake ..
cpack

# Go ahead and install the DEB.
ls *.deb
sudo dpkg -i hello-world-0.1-Linux.deb

# Check the status of the service - should be stopped.
systemctl status hello-world

# Start the service, then check its status.
systemctl start hello-world
systemctl status hello-world

# Removing the RPM should cleanup all traces.
sudo apt purge hello-world
systemctl status hello-world
```

##  Debian
Try to add support for .deb packages
