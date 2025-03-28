# You can provision an NGINX server in less than a minute using Docker on Mac, Windows, or Linux. 

Download Docker Desktop for Your OS I use Linux OS so I will show you the steps to do that if you are  
using the same OS as mine which is Kali Linux these steps are for you let's do this.

## Install Docker Using Kali’s Default Repositories

Since Docker is included in Kali’s repositories, you can install it without adding external sources.  

**Step 1:** Update Your System  

`sudo apt update && sudo apt upgrade -y`

**Step 2:** Install Docker from Kali's Repository  

`sudo apt install -y docker.io`

**Step 3:** Start & Enable Docker  
```
sudo systemctl start docker
sudo systemctl enable docker
```
**Step 4:** Verify Docker Installation  

Check if Docker is installed correctly:

`docker --version`

Expected Output (Example)  

`Docker version 20.10.21, build a89b842`

Run a test container:  

`sudo docker run hello-world`  

Step 5: Run Docker Without Sudo  

To allow running Docker without sudo:  

```
sudo usermod -aG docker $USER
newgrp docker
```

Now, restart your terminal and try running: 

`
docker run hello-world
`  

That's It! Docker is now installed on Kali Linux without external repositories.  

 
Create a directory named `learn-terraform-docker-container`.  

`
$ mkdir learn-terraform-docker-container
`  

This working directory houses the configuration files that you write to describe the infrastructure  
you want Terraform to create and manage. When you initialize and apply the configuration here, Terraform  
uses this directory to store required plugins, modules (pre-written configurations), and information about  
the real infrastructure it created.

Navigate into the working directory.

`
$ cd learn-terraform-docker-container
`  

In the working directory, create a file called `main.tf` and paste the following Terraform configuration into it.  

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
```

Initialize the project, which downloads a plugin called a provider that lets Terraform interact with Docker.   

`
$ terraform init
`

Provision the NGINX server container with apply. When Terraform asks you to confirm type yes and press ENTER.

`
$ terraform apply
`

Verify the existence of the NGINX container by visiting [localhost:8000](https://localhost:8000) in your web browser or running docker    
ps to see the container.  

NGINX running in Docker via Terraform   

![Screenshot_2025-03-28_16-26-18](https://github.com/user-attachments/assets/cd6f47bf-1ff3-4e9d-b32e-312bf7e7a586)


To stop the container, run     

`
$ terraform destroy
`

You've now provisioned and destroyed an NGINX webserver with Terraform  
