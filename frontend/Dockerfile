# Use an official Node.js base image (includes npm)
FROM node:16

# Set the working directory inside the container
WORKDIR /app
RUN chown -R node:node /app

# Copy package.json and package-lock.json first to leverage Docker's cache
COPY package*.json .

# Install the necessary dependencies (Vite and other dependencies)
RUN npm install

# Copy the rest of the frontend source code to the container
COPY . .

# Expose the port on which the frontend app will run
EXPOSE 5173

# Run the frontend app in development mode using Vite
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]