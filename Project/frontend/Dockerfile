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
