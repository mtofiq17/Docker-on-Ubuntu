### Installing Docker
The Docker installation package available in the official Ubuntu repository may not be the latest version. To ensure we get the latest version, we’ll install Docker from the official Docker repository. To do that, we’ll add a new package source, add the GPG key from Docker to ensure the downloads are valid, and then install the package.

First, update your existing list of packages:

```sudo apt update```

Next, install a few prerequisite packages which let apt use packages over HTTPS:

```sudo apt install apt-transport-https ca-certificates curl software-properties-common```

Then add the GPG key for the official Docker repository to your system:

```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```

Add the Docker repository to APT sources:

```sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"```

Next, update the package database with the Docker packages from the newly added repo:

```sudo apt update```

Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:

```apt-cache policy docker-ce```

You’ll see output like this, although the version number for Docker may be different:

```
ocker-ce:
  Installed: (none)
  Candidate: 18.03.1~ce~3-0~ubuntu
  Version table:
     18.03.1~ce~3-0~ubuntu 500
        500 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
```

Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 18.04 (bionic).

Finally, install Docker:

```sudo apt install docker-ce```

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:

```sudo systemctl status docker````

If your user isn't in the docker group, add it:

```sudo usermod -aG docker $USER```

Wide Open Permissions: Setting the permissions to 666 grants read and write access to all users on the system. This means any user, even those without administrative privileges, could potentially manipulate Docker containers.

```sudo chmod 666 /var/run/docker.sock ```

### Installing Docker Single Bash Command

```
#!/bin/bash

# Update your existing list of packages
sudo apt update

# Install prerequisite packages which let apt use packages over HTTPS
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# Add the GPG key for the official Docker repository to your system
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add the Docker repository to APT sources
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Update the package database with the Docker packages from the newly added repo
sudo apt update

# Install Docker
sudo apt install -y docker-ce

# Check that Docker is running
sudo systemctl status docker

# Add your user to the docker group (requires logout and login to take effect)
sudo usermod -aG docker $USER

# Print completion message
echo "Docker installation completed. Please logout and log back in to apply group changes."
```
