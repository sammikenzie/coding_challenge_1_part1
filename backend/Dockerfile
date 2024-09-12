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
