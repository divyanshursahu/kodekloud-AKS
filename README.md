# Lab Guide 
### Building and containazing a sample application

In this lab, you will learn how to deploy a sample ASP.NET Core Web App or a Python Web App on a local Kubernetes cluster using Docker. The lab consists of the following sections:

1. Prerequisites:

    - Docker installed on your local machine.
    - Kubernetes (kubectl) installed on your local machine.
    - .NET Core SDK installed (for the ASP.NET Core Web App).
    - Python installed (for the Python Web App).
    - Git installed.

2. Setting up the Local Kubernetes Cluster:

     - Install a local Kubernetes cluster such as Minikube or Docker Desktop with Kubernetes enabled.
     - Start the Kubernetes cluster and ensure it is running properly.

3. Building the Sample ASP.NET Core Web App:
     - Open a terminal or command prompt.
     - Clone the ASP.NET Core Web App repository from GitHub:

  ```git clone https://github.com/yourusername/your-aspnet-webapp.git```

https://github.com/hpranav/kodekloud/tree/main/AKS/KodeKloudApp

- Navigate to the project directory:
  
```cd your-aspnet-webapp```

- Build the ASP.NET Core Web App:

```dotnet build```

- Run the application locally:

```dotnet run```

- Verify that the application is running correctly by accessing http://localhost:5000 in your web browser.

4. Building the Sample Python Web App:
   - Open a terminal or command prompt.
   - Clone the Python Web App repository from GitHub:
```git clone https://github.com/yourusername/your-python-webapp.git```

- Navigate to the project directory:

```cd your-python-webapp```

- (Optional) Set up a virtual environment:

```python -m venv venv```

```source venv/bin/activate (Linux/Mac) OR v env\Scripts\activate (Windows)```

- Install the required dependencies:
```pip install -r requirements.txt```

- Run the Python Web App locally:
```python app.py```

- Verify that the application is running correctly by accessing http://localhost:5000 in your web browser.

5. Containerizing the Web App:

   - Choose either the ASP.NET Core Web App or the Python Web App for containerization.

For ASP.NET Core Web App:

- Create a Dockerfile in the root of your project directory with the following content:

```
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR /app

COPY . .

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS runtime

WORKDIR /app

COPY --from=build /app/out .

ENTRYPOINT ["dotnet", "webapp.dll"]
```

- Build the Docker image:

```docker build -t your-aspnet-webapp .```

Verify that the Docker image was created successfully:
```docker images```

## For Python Web App:

1. For Python Web App:
   - Create a Dockerfile in the root of your project directory with the following content:

```
FROM python:3.9

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]
```

- Build the Docker image:

```docker build -t your-python-webapp .```

- Verify that the Docker image was created successfully:
```docker images```

2. Running the Web App on the Local Kubernetes Cluster:

     - Ensure that your local Kubernetes cluster is running and you have the kubectl command-line tool installed.
     - Deploy the Web App to the local Kubernetes cluster:

```kubectl create deployment your-webapp --image=your-aspnet-webapp```(for ASP.NET Core)

OR 

```kubectl create deployment your-webapp --image=your-python-webapp```(for Python)

- Expose the deployment as a service:

```kubectl expose deployment your-webapp --port=80 --target-port=5000 --type=LoadBalancer```

- Retrieve the external IP address of the service:

```kubectl get service your-webapp```

- Access the application by using the external IP address and port 80 in your web browser.
  
Congratulations! You have successfully completed the lab and deployed either the ASP.NET Core Web App on your local Kubernetes cluster using Docker.

**Explanation of Commands:**

- **git clone:** This command clones a Git repository and creates a local copy of the repository on your machine.
- **cd:** This command is used to change the current directory in the command prompt or terminal.
- **dotnet build:** This command compiles the ASP.NET Core Web App source code and generates the necessary binaries.
- **dotnet run:** This command runs the ASP.NET Core Web App locally.
- **python app.py:** This command runs the Python Web App locally.
- **docker build:** This command builds a Docker image based on the Dockerfile in the current directory.
- **docker images:** This command lists the Docker images available on your local machine.
- **kubectl create deployment:** This command creates a deployment in Kubernetes, which defines the desired state for the application.
- **kubectl expose deployment:** This command creates a service in Kubernetes, which provides networking and load balancing for the deployment.
- **kubectl get service:** This command retrieves information about the services in Kubernetes, including the external IP address for accessing the application.
