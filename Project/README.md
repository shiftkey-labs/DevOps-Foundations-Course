# Multi-Container Docker Application with CI/CD: Calculator App Project

## Project Overview
The goal of this project is to assess your understanding of Docker containerization and Continuous Integration/Continuous Deployment (CI/CD) principles. You will be provided with a simple calculator web application consisting of a **React frontend** and a **Python backend**. Your task is to containerize these components, create a multi-container setup using Docker Compose, and implement an automated CI/CD pipeline.

## Learning Objectives
After successful completion of this project, you will:
1. Gain practical experience in containerizing existing applications using Docker.
2. Learn to orchestrate multi-container applications with Docker Compose.
3. Understand and implement CI/CD practices using GitHub Actions or GitLab CI/CD.
4. Develop skills in writing clear documentation and setup instructions.

## Provided Components
The following code will be available:
1. **Frontend:** A React-based calculator web application.
2. **Backend:** A Python Flask API for performing calculation operations.

> **NOTE:** The template `Dockerfile` and `YAML configurations` are available in this directory. It is **NOT** compulsory to use these files as they are. It may be that creating your own files from scratch and referring to the templates may be more beneficial and efficient. Feel free to adapt based on which feels best as needed.

## TODO Tasks
### STEP-1: Implement the Dockerfiles
1. Create Dockerfiles for both frontend and backend components.
2. Develop a `docker-compose.yml` file for local development.
3. (Good to have) Use environment variables for configuration management.

### STEP-2: Implement the CI/CD Pipeline
1. Implement a workflow using GitHub Actions or GitLab CI/CD.
2. Automate the building and testing of Docker images.
3. Configure the pipeline to push images to Docker Hub.

### STEP-3: Write Clear Documentation
- Create a comprehensive README.md with setup instructions and an explanation of the created Dockerfile and YAML configuration.

## Technical Requirements

### 1. Docker Implementation
#### i. Frontend Dockerfile:
- Use an appropriate base image (e.g., `node:14-alpine`).
- Copy the entire frontend code along with `package.json`.
- Install dependencies in `package.json`.
- Build the React app.
- Start running the React app using `npm`.
- (Optional) Use a lightweight web server (e.g., `nginx:alpine`) to serve the built files.

#### ii. Backend Dockerfile:
- Use an appropriate base image (e.g., `python:3.9-slim`).
- Copy `requirements.txt` and install dependencies.
- Copy the rest of the backend code.
- Expose the necessary port.
- Specify the command to run the API server.

#### iii. docker-compose.yml:
- Define services for both frontend and backend.
- Map appropriate ports.
- Set up environment variables.
- Ensure proper networking between services.

#### iv. (Optional) Environment Variables:
- Use `.env` files for local development.
- Implement environment variables for sensitive information (e.g., API endpoints).

### 2. CI/CD Pipeline (GitHub Actions)
Create a YAML configuration file with the following steps:
1. Trigger on push to the main branch and pull requests.
2. Set up environment (Node.js, Python, Docker).
3. Build Docker images for frontend and backend.
4. Run any tests (Optional).
5. Push images to Docker Hub (on successful merge to main).

## Testing Requirements

### Application Testing (Frontend + Backend)
1. **API Endpoint Validation**
   - Ensure all calculation endpoints are reachable & return correct results.

2. **Functional Testing**
   - Verify all calculator operations work correctly:
     - Addition (+)
     - Subtraction (-)
     - Multiplication (*)
     - Division (/)

### Docker and Containerization Testing
1. **Compose File Validation**
   - Verify containers start successfully on appropriate ports.
   - Check if frontend-container is successfully communicating with backend-container.
   - Validate port mappings.

## Project Timeline and Submission

### Important Dates
<!-- - **Project Assigned:** November 15, 2024 -->
- **Submission Deadline:** December 4, 2024, 11:59 PM
- **Submission Method:** Online submission form 

> NOTE: (_Form link will be available soon as deadline approaches._)

### Submission Checklist
- [ ] Frontend Dockerfile
- [ ] Backend Dockerfile
- [ ] docker-compose.yml
- [ ] CI/CD YAML configuration file
- [ ] README.md
- [ ] Docker Hub image links
- [ ] Successful functional testing

## Project Deliverables
1. `Dockerfile` for frontend.
2. `Dockerfile` for backend.
3. `docker-compose.yml` file.
4. CI/CD YAML configuration `.github/workflows/ci.yml` OR `.gitab-ci.yml` file.
5. `README.md` file containing:
   - Brief Project overview.
   - Instructions for setting up and running the application locally using Docker Compose.
   - Explanation of the CI/CD pipeline and its stages.
   - Any assumptions or design decisions made.

## Submission Guidelines
1. Fork the provided [GitHub repository](https://github.com/shiftkey-labs/DevOps-Foundations-Course/tree/master) containing the frontend and backend code (present in the Project folder).
2. Implement your solutions in the forked repository.
3. Ensure your repository includes all required files:
     - Dockerfiles
     - docker-compose.yml 
     - CI/CD YAML congiguration file 
4. Push your Docker images to Docker Hub.
5. The form to submit the following will be available soon:
   - URL or link of your GitHub repository.
   - URL or link of your Docker Hub repository.
   

## Evaluation Criteria
Your project will be evaluated based on the following:
1. Correct implementation of `Dockerfiles` and `docker-compose.yml` (**40%**).
2. Successful implementation of the CI/CD pipeline using GitHub Actions or GitLab CI/CD (**40%**).
3. Completeness and clarity of the README documentation (**20%**).

## Support
If you encounter any issues or have questions about the project requirements, Docker concepts, or CI/CD implementation, please reach out to the instructor for clarification and assistance.


---
