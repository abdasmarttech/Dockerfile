# Use the official Jenkins LTS image from Docker Hub
FROM jenkins/jenkins:lts

# Set environment variables for Jenkins

ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

# Switch to the root user to install additional packages
USER root

# Update the package list and install any necessary packages
RUN apt-get update -y

  # Install Apache2
sudo apt install apache2 -y

# Enable and start Apache2 service
sudo systemctl enable apache2
sudo systemctl start apache2

# Install prerequisites for Docker
sudo apt install -y ca-certificates curl gnupg

# Add Docker’s official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.gpg > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Set up Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package list
sudo apt update -y

# Install Docker Engine
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Enable and start Docker service
sudo systemctl enable docker
sudo systemctl start docker

# Install Blue Ocean plugin
RUN jenkins-plugin-cli --plugins blueocean

# Verify installation
docker --version
apache2 -v
    

# Create a directory for Jenkins data (optional, for persistent storage)
RUN mkdir -p /var/jenkins_home

# Set the correct permissions for the Jenkins user
RUN chown -R jenkins:jenkins /var/jenkins_home

# Start Jenkins by default when the container runs
ENTRYPOINT ["/usr/bin/tini", "--", "/usr/local/bin/jenkins.sh"]
    
# Allow Jenkins user to use sudo without a password
RUN echo "jenkins ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Switch back to the Jenkins user
USER jenkins

# Expose port 8080 for the Jenkins web interface
EXPOSE 8080

# Expose port 60000 for Jenkins agent connections
EXPOSE 70000

# Start Jenkins
CMD ["jenkins"]
