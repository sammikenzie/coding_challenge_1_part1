1. Install the required tools:
   - Node.js | We need this tool to run the backend (express) & frontend (react)
   - Npm | This tool helps us manage Javascript libraries & dependencies
   - Terraform | Is an IaC (Infrastructure as Code) tool to automate the provisioning of our cloud resources, such as the AWS ECS, VPC, EC2, Subnets & etc
   - Docker | Will allow us to package the frontend and backend into containers & Amazon ECS (Elastic Container Service) will use these containers for deployment
   - AWS CLI | This will allow us to interact with AWS from the command line

2. Setup the Backend & Frontend
   1. Download the challenge code, this will give you the React Frontend & Express Backend that you need to deploy.
      - Clone the repository: git clone https://github.com/TayoLusi19/devops-code-challenge1.git
   2. Setup the Backend (Express.js). The Backend will handle API requests from the frontend. This is how two systems communicate with each other over the internet.
      Usually involves sending data or requesting services from one system to another. In our case, the front end will request the back end to fetch the data for the user.
    - cd into Backend folder: cd backend
    - Install dependencies using npm: npm ci
    - Start the backend server: npm start
    The backend should now be running on Port: 8080
   3. Setup the Frontend (React.js). The Frontend will call the Backend to fetch data so that the user can interact with it.
    -  cd into Frontend folder: cd frontend
    -  Install dependencies: npm ci
    -  Start frontend server: npm start
    The frontend should now be running on Port: 3000

3. Dockerize the Applications
   1. Create Dockerfiles for both applications - We are doing this because Docker allows us to package our applications so they can run in any environment consistently.
     - Create a Dockerfile in the Backend Folder (This file will tell Docker how to build a container for the backend). Copy & Paste the code into the Dockerfile:
           # Use the Node.js 16 image as the base
          FROM node:16
          
          # Create and set the working directory inside the container
          WORKDIR /app
          
          # Copy the package files to install dependencies
          COPY package*.json ./
          
          # Install dependencies
          RUN npm ci
          
          # Copy the rest of the backend code
          COPY . .
          
          # Expose port 8080
          EXPOSE 8080
          
          # Start the application
          CMD ["npm", "start"]

      - Create a Dockerfile in the Frontend Folder (This will tell Docker how to build a container for the frontend). Copy & Paste the code into the Dockerfile:
          # Use Node.js as the base image
          FROM node:16
          
          # Set the working directory
          WORKDIR /app
          
          # Install dependencies
          COPY package*.json ./
          RUN npm ci
          
          # Copy the frontend code
          COPY . .
          
          # Expose port 3000
          EXPOSE 3000
          
          # Start the app
          CMD ["npm", "start"]
    2. Build & Run the Docker Containers - We do this to confirm that the containers work properly before deploying them to AWS
     -  Build the backend container:
       docker build -t backend-app ./backend
     - Build the frontend container:
       docker build -t frontend-app ./frontend
     - Run the containers locally:
       docker run -p 8080:8080 backend-app
       docker run -p 3000:3000 frontend-app
     To test the applications, open a web browser and confirm that the backend is responding: http://localhost:8080 & confirm that the frontend works and successfully connects to the backend: http://localhost:3000

 4. 













































       
