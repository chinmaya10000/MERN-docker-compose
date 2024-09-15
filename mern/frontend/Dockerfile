# # Stage 1: Build Stage
FROM node:18.9.1 AS frontend-builder

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install Dependencies
RUN npm install

# Copy the rest of application code
COPY . .

# Stage 2
FROM node:18.9.1-slim

# Set the working directory
WORKDIR /app

# Copy only the built files from the build stage
COPY --from=frontend-builder /app /app

# Expose the port your app runs on
EXPOSE 5173

# Command to run application
CMD ["npm", "run", "dev"]