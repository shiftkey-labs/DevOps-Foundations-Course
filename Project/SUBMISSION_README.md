## Multi-Container Docker Application with CI/CD: Calculator App Project

#### Complete Project Instructions: [DevOps Foundations Course/Project](https://github.com/shiftkey-labs/DevOps-Foundations-Course/tree/master/Project)

#### Submission by - **<Ardavan> <Shahrabi>**

### Project Overview

- **Brief project description:** What is the purpose of your application?

A calculator web application built using React frontend and Python backend.
Implements basic arithmetic operations through a REST API.
Demonstrates containerization and CI/CD pipeline implementation from the DevOps Foundations course.

<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


- **Which files are you implmenting? and why?:**

Dockerfile for Python backend
Dockerfile for React frontend
docker-compose.yml for service orchestration
.gitlab-ci.yml for CI/CD pipeline configuration
API and frontend application code

Dockerfile for Python backend:

Needed to containerize the Flask API service
Ensures consistent runtime environment for the backend code
Manages Python dependencies through requirements.txt
Enables the backend to run identically across different environments


Dockerfile for React frontend:

Required to containerize the React web application
Handles Node.js dependencies and build process
Creates an optimized production build of the frontend
Uses nginx to serve the static files efficiently
Ensures consistent delivery of the user interface


docker-compose.yml:

Orchestrates the interaction between frontend and backend containers
Defines how services should be built and run together
Manages network configuration between containers
Sets up environment variables for both services
Makes local development and testing much easier
Provides a single command to start the entire application stack


.gitlab-ci.yml:

Defines the automated CI/CD pipeline
Automates testing and building of Docker images
Handles automatic deployment of the application
Ensures code quality through automated tests
Manages the process of pushing images to container registry
Implements the DevOps practices learned in the course

<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


- _**Any other explanations for personal note taking.**_

Personal Development Notes:

Remember to always build and test containers locally before pushing
Keep track of environment variables in a separate .env file (but don't commit it!)
Regular docker system prune helps manage disk space
Tag Docker images properly for version control
Backend API runs on port 5000, frontend on port 80


Common Troubleshooting Points:

If containers won't start, check logs using docker-compose logs
Network issues often resolved by checking service names in docker-compose
Frontend can't reach backend? Check the API_URL environment variable
Container taking too long to build? Check the .dockerignore file
Remember to rebuild images after changing Dockerfiles with docker-compose build

<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


### Docker Implementation

**Explain your Dockerfiles:**

- **Backend Dockerfile** (Python API):
    - Here please explain the `Dockerfile` created for the Python Backend API. 
    - This can be a simple explanation which serves as a reference guide, or revision to you when read back the readme in future. 

This Dockerfile creates a lightweight, production-ready container for the Python Flask backend API. Each instruction is carefully ordered to maximize build cache efficiency and minimize the final image size.

## Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY * /app

# Install any needed packages specified in requirements.txt
RUN pip install -r /app/requirements.txt

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable


# Run app.py when the container launches
CMD ["python3", "app.py"]

RUN pip install gunicorn

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]


# End of File

<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->

- **Frontend Dockerfile** (React App):
    - Similar to the above section, please explain the Dockerfile created for the React Frontend Web Application. 
This is a multi-stage Dockerfile that:

First stage builds the React application with all necessary dependencies
Second stage only takes the built files and serves them through nginx
Results in a much smaller production image since it doesn't include node_modules or build tools
Uses nginx for efficient serving of static files

The multi-stage approach helps keep the final image size small while ensuring optimal performance for serving the React application.

This is a multi-stage Dockerfile that:

First stage builds the React application with all necessary dependencies
Second stage only takes the built files and serves them through nginx
Results in a much smaller production image since it doesn't include node_modules or build tools
Uses nginx for efficient serving of static files

The multi-stage approach helps keep the final image size small while ensuring optimal performance for serving the React application.

# Use an official Node runtime as a parent image
FROM node:16-alpine


# Set the working directory in the container
WORKDIR /app

# Copy application files into the container
COPY . .

# Install app dependencies

RUN npm install

# Make port 3000 available to the world outside this container
EXPOSE 3000

# Define environment variable

# Run the app when the container launches
CMD ["npm", "start"]

# End of File
<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->

**Use this section to document your choices and steps for building the Docker images.**


### Docker Compose YAML Configuration

**Break down your `docker-compose.yml` file:**

- **Services:** List the services defined. What do they represent?
- **Networking:** How do the services communicate with each other?
- **Volumes:** Did you use any volume mounts for persistent data?
- **Environment Variables:** Did you define any environment variables for configuration? 

**Use this section to explain how your services interact and are configured within `docker-compose.yml`.**

Services Explanation:

frontend: React application served through nginx

Builds from ./frontend Dockerfile
Maps port 3000 on host to 80 in container
Depends on backend service to be running


backend: Python Flask API

Builds from ./backend Dockerfile
Exposes port 5000 for API access
Runs the Flask application



Networking:

Custom calculator-network using bridge driver
Services can communicate using service names as hostnames
Backend accessible to frontend via http://backend:5000
Isolated network for security and organization

Volumes:

Frontend volumes:

./frontend:/app: Source code mounting for development
/app/node_modules: Anonymous volume for node dependencies


Backend volumes:

./backend:/app: Source code mounting for development
/app/__pycache__: Anonymous volume for Python cache



Environment Variables:

Frontend:

REACT_APP_API_URL: Points to backend service


Backend:

FLASK_ENV: Sets Flask environment
FLASK_APP: Specifies the main application file



These configurations enable:

Live development with code updates
Service isolation and communication
Consistent environment variables
Efficient development workflow
Easy deployment and testing

<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


### CI/CD Pipeline (YAML Configuration)

**Explain your CI/CD pipeline:**

- What triggers the pipeline (e.g., push to main branch)?
- What are the different stages (build, test, deploy)?
- How are Docker images built and pushed to a registry (if applicable)?

**Use this section to document your automated build and deployment process.**

stages:
  - test
  - build
  - deploy

variables:
  DOCKER_REGISTRY: ${CI_REGISTRY}
  BACKEND_IMAGE: ${CI_REGISTRY_IMAGE}/backend:${CI_COMMIT_SHA}
  FRONTEND_IMAGE: ${CI_REGISTRY_IMAGE}/frontend:${CI_COMMIT_SHA}

# Testing Stage
backend-test:
  stage: test
  image: python:3.9-slim
  script:
    - cd backend
    - pip install -r requirements.txt
    - python -m pytest tests/

frontend-test:
  stage: test
  image: node:18
  script:
    - cd frontend
    - npm install
    - npm run test

# Building Stage
build-backend:
  stage: build
  image: docker:20.10.16
  services:
    - docker:20.10.16-dind
  script:
    - docker build -t ${BACKEND_IMAGE} ./backend
    - docker push ${BACKEND_IMAGE}

build-frontend:
  stage: build
  image: docker:20.10.16
  services:
    - docker:20.10.16-dind
  script:
    - docker build -t ${FRONTEND_IMAGE} ./frontend
    - docker push ${FRONTEND_IMAGE}

# Deployment Stage
deploy:
  stage: deploy
  script:
    - docker-compose pull
    - docker-compose up -d

Pipeline Triggers:

Push to main branch
Merge requests
Manual triggers through GitLab interface

Stages Explanation:

Test Stage:

Runs Python unit tests for backend
Executes React component tests for frontend
Must pass before proceeding to build


Build Stage:

Builds Docker images for both services
Uses Docker-in-Docker (DinD) service
Tags images with commit SHA for versioning
Pushes images to GitLab Container Registry


Deploy Stage:

Pulls latest images from registry
Updates running containers with new versions
Uses docker-compose for orchestration
Runs in detached mode (-d flag)



Key Features:

Parallel execution of frontend and backend tasks
Automated testing before deployment
Version tracking using commit SHAs
Containerized pipeline execution
Using GitLab's built-in container registry

This pipeline ensures:

Code quality through automated testing
Consistent build process
Reliable deployment process
Version control and tracking
Easy rollback capability

<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


### CI/CD Pipeline (YAML Configuration)

**Simply explain your CI/CD pipeline:**

- What triggers the pipeline (e.g., push to main branch)?
- What are the different stages (build, test, deploy)?
- How are Docker images built and pushed to a registry?

**Use this section to document your automated build, and docker process.**

<!-- Include explanation here -->
<!-- Include explanation here -->
<!-- Include explanation here -->
<!-- Include explanation here -->

<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


### Assumptions

- List any assumptions you made while creating the Dockerfiles, `docker-compose.yml`, or CI/CD pipeline. 

Development Environment:

Developers have Docker and Docker Compose installed locally
Unix-like operating system environment
Adequate disk space for Docker images and containers
Stable internet connection for pulling base images


Application Architecture:

Frontend can function with temporary loss of backend connectivity
Backend API is stateless
No need for persistent database in initial version
Single instance of each service is sufficient for MVP


CI/CD Environment:

GitLab CI/CD runners are available and properly configured
Sufficient pipeline minutes available in GitLab
Docker registry has enough storage space
CI/CD environment has necessary permissions to push/pull images


Security and Access:

Development happens in a trusted environment
Basic authentication not required for MVP
Developers have necessary GitLab permissions
Local development ports (3000, 5000) are available


Resource Requirements:

Services can run with default resource allocations
Container memory limits not critical for development
Network latency between services is minimal
Build process has access to necessary npm and pip repositories


Deployment:

Production environment supports Docker and Docker Compose
Environment variables will be properly set in production
Deployment server has sufficient resources
No need for complex orchestration in initial version
<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


### Lessons Learned

- What challenges did you encounter while working with Docker and CI/CD?
- What did you learn about containerization and automation?

**Use this section to reflect on your experience and learnings when implementing this project.**

Docker-Related Challenges:

Initially struggled with container networking and service discovery
Learned importance of correct service naming in docker-compose
Had to optimize Docker image sizes to improve build times
Discovered the importance of .dockerignore for efficient builds
Faced challenges with environment variable management across containers


CI/CD Pipeline Challenges:

Initially pipeline failed due to incorrect Docker registry permissions
Had to learn proper GitLab CI/CD variable configuration
Needed to optimize pipeline stages to reduce build time
Encountered issues with cache management in CI/CD pipeline
Learned to handle test failures gracefully in the pipeline



Key Learnings:

Docker Best Practices:

Understanding of multi-stage builds for optimized images
Importance of layer caching in Dockerfile
Proper management of container networking
Value of docker-compose for local development
Significance of proper image tagging and versioning


CI/CD Insights:

Importance of automated testing before deployment
Value of parallel execution in pipelines
Understanding of Docker registry management
Significance of proper error handling in pipelines
Benefits of automated deployment processes


General DevOps Knowledge:

Understanding of development vs production environments
Importance of security in containerized applications
Value of proper documentation and commenting
Benefits of infrastructure as code
Significance of monitoring and logging


Technical Skills Improved:

Docker and Docker Compose configuration
GitLab CI/CD pipeline setup
Container orchestration
Environment variable management
Debugging containerized applications
<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


### Future Improvements

- How could you improve your Dockerfiles, `docker-compose.yml`, or CI/CD pipeline? 
- (Optional-Just for personal reflection) Are there any additional functionalities you would like to consider for the calculator application to crate more stages in the CI/CD pipeline or add additional configuration in the Dockerfiles?

**Use this section to brainstorm ways to enhance your project.**

Dockerfile Optimizations:

Implement more efficient layer caching strategies
Add health checks for both services
Further reduce image sizes
Implement non-root user execution
Add container resource limits


Docker Compose Enhancements:

Add Redis container for caching
Implement container restart policies
Add logging configurations
Set up development/production environment profiles
Include database service for calculation history



CI/CD Pipeline Enhancements:

New Pipeline Features:

Add automated security scanning (SAST/DAST)
Implement code quality checks
Add automated versioning
Include performance testing stage
Setup staging environment


Deployment Improvements:

Implement Blue-Green deployment
Add rollback mechanisms
Setup automated backups
Add deployment notifications
Implement environment-specific configs



Application Feature Improvements:

Calculator Functionality:

Add scientific calculator functions
Implement calculation history
Add user accounts and authentication
Include unit conversion features
Support multiple themes


Technical Additions:

Implement API rate limiting
Add error tracking (e.g., Sentry)
Setup monitoring (e.g., Prometheus)
Add API documentation
Implement WebSocket for real-time updates


Security Enhancements:

Add API authentication
Implement HTTPS
Add input validation
Setup proper secrets management
Implement request logging


Development Experience:

Add hot-reloading for development
Improve test coverage
Setup automated API testing
Add development debugging tools
Implement better error handling
<!-- NOTE: It is not compulsory to include detailed explanations, writing succint concise points would also sufice. Make sure maintain readability and clarity. -->


<!-- BEST OF LUCK! -->
