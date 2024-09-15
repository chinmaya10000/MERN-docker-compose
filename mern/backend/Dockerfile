# Stage 1: Build stage
FROM node:18.9.1 AS backend-builder

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install Dependencies
RUN npm install 

# Copy the rest of application code
COPY . .

# Stage 2:
FROM node:18.9.1-slim

# Set the working directory
WORKDIR /app

# Copy only the necessary files from the build stage
COPY --from=backend-builder /app /app/

# Expose the port your app runs on
EXPOSE 5050

# Command to run application
CMD ["npm", "start"]
