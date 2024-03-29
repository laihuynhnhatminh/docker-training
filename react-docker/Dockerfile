# set the base image to create the image for react app
FROM node:20-alpine

# create a user with permissions to run the app
# -S -> create a system user
# -G -> add the user to a group
# This is done to avoid running the app as root
# If the app is run as root, any vulnerability iun the app can be exploited to gain access to the host system
# It's a good practive to run the app as a non-root user
RUN addgroup app && adduser -S -G app app

# set the user to run the app
USER app

# set the working directory
WORKDIR /app

# copy package.json and package-lock.json to the working directory
# This is done before copying the rest of the files to take advantage of Docker's cache
# If the package.json and package-lock.json files haven't changed, Docker will use the cached dependencies
# This will speed up the build process
COPY package*.json ./

# sometimes the ownership of the files in the working directory is changed to root
# and this the app can't access thje files and throw an error => EACCES: permission denied
# to avoid this, change the ownership of the files to the root user
USER root

# change the owneshio of the /app directory to the app user
# chown -R <user>:<group> <directory>
# chown command changes the user and/or group ownership for given files.
# -R -> change the ownership of the directory and all its contents
# could also be written as chown -R app:app /app
RUN chown -R app:app .

# Change the user back to the app user
USER app

# install the dependencies
RUN npm install

# copy the rest of the files to the working directory
COPY . .

# Expost port 5173 to tell Docker that the container listens on the specified network ports at runtime
EXPOSE 5173

# start the app in development mode
# we can also build the app for production and start the app in production mode
# RUN npm run build
# CMD npm start:prod
CMD npm run dev