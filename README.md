# How to deploy a simple getting started asp.net core web api to Azure Kubernetes Service
In this tutorial we will go over the simple steps on How to deploy a simple getting started asp.net core api to Azure Kubernetes Service. We would start with the tools required on your local machine and along with that the kind of services that you need to provision on Azure in order to get it working.
## Local Installations
We have to first install the latest version of Docker Desktop as this makes containerizing and managing those containers very easy. So you can download the latest version of docker from https://www.docker.com/products/docker-desktop. I am using a Windows 10 machine so I downloaded the Docker Dektop for windows. After running the installation you check the latest version with the following command
docker --version
Mine was showing as Docker version 20.10.7, build f0df350
Once docker in installed, there is another requisite that needs to be installed which is a version of Visual Studio (community or professional). Almost the same way it will also work with Visual Studio Code. Once you have installed the latest version of Visual Studio (https://visualstudio.microsoft.com/downloads/) along with the latest version of dotnet core you can check the version in the command prompt using the command : dotnet --version. Mine is 5.0.301.
## Create the getting started project and adding docker support
First create a VS Studio Asp.NET Core API project called Hello.api (.NET Core 5, C#), it will create a Weather Forecast Controller without docker support. After running it on Swagger and hitting a debug point. right click on the project and select "Add" and then add "Docker Support". The default debug option will change to "Docker" and you can now run the project in docker with the same debug experience as in Visual studio. Visual Studio provides excellent features for docker support but here are a list of handy docker commands below
> docker ps
For inspecting the docker container you can use
> docker inspect <id of container>
For checking resource usage of the container use
> docker stats <id of the container>
For checking logs for a container you can use
> docker logs <id of the container>
Once you run the project in Visual Studio with docker support it will automatically build the local docker image, once you do docker ps or docker images you will see an image like this below
>docker images
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
helloapi                          latest    d62605592f0c   39 seconds ago   209MB 
## Tag local image and push to container registry
I will be using the docker hub as my container registry and the regustry will be public so that our kubernetes service can pull the image from that registry. You can also select Azure container registry as explained in this article from Microsoft : https://docs.microsoft.com/en-us/dotnet/architecture/containerized-lifecycle/design-develop-containerized-apps/build-aspnet-core-applications-linux-containers-aks-kubernetes , we are skipping the Azure container registry as of now because our focus is to deploy on a kubernetes service from any registry. In order to tag the local image and push to the container registry please follow the steps below
  1. First Login to Docker Hub >docker login -u "<dockerHubUser>" -p "<dockerHubPassword>" docker.io
  2. On the docker hub website we create a repository called "helloapi"
  3. Here is the format below to first tag a locally build image to a new repository and tag and then push
     > docker tag local-image:tagname new-repo:tagname
     > docker push new-repo:tagname
  So in my case it was like below
  >docker tag helloapi:latest kaushikghosh123/helloapi:latest
  >docker push kaushikghosh123/helloapi:latest
  
Once the image is pushed the same would show up on the docker hub website.
