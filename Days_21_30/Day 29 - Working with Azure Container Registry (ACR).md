The Nautilus DevOps team has been tasked with setting up a containerized application. They need to create a Azure Container Registry (ACR) to store their Docker images. Once the repository is created, they will build a Docker image from a Dockerfile located on the `azure-client` host and push this image to the ACR repository. This process is essential for maintaining and deploying containerized applications in a streamlined manner.

1. Create a ACR repository name `datacenteracr22499` under `East US`.
2. Pricing plan must be `Basic`.
3. Dockerfile already exists under `/root/pyapp` directory on `azure-client` host.
4. Build a Docker image using this Dockerfile and push the same to the newly created ACR repo. The image tag must be `latest` i.e `datacenteracr22499:latest`.

`Notes`:
- Create the resource only in `East US` region.

### Answer using Azure Portal

#### Create ACR repository

1. In the top search bar, search for container registries and click on Container registries under Services. 
2. Press + Create.
3. Assign the existing resource group to the registry.
4. Under name use the `datacenteracr22499`.
5. Ensure Region is `East US`.
6. Ensure Pricing plan is `Basic`.
7. The rest can remain default and press Review + Create.
8. Press Create and wait for the registry to be deployed.
#### Build Docker image and push to ACR

1. In the `azure-client` CLI, go to the `/root/pyapp` repository.
2. Build the docker image with the following command ;

```
docker build . -t datacenteracr22499:latest
```

- . = current directory, where the Dockerfile is located
- -t = TAG (name:TAG)

3. Once the image is build it can be shown via the command;

```
docker image list
```

#### Push image to ACR

1. First login to the ACR using the following command;

```
az acr login --name datacenteracr22499
```

2. In order for the image to have the correct tag in the ACR we have to tag the image with the as follows;

```
docker tag datacenteracr22499:latest datacenteracr22499.azurecr.io/datacenteracr22499
```

The `datacenteracr22499.azurecr.io` is the Login server address, which is found in the overview of the ACR. 

3. Once tagged we can push the image as follows;

```
docker push datacenteracr22499.azurecr.io/datacenteracr22499:latest
```

4. Once the push is complete check in the ACR under Services - Repository if the image is available. 
5. Clicking on the image will reveal the Tags.
6. Click check to complete the task. 
