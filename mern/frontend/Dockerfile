#using official image as parent imager 
FROM node:18.9.1

#set working directory
WORKDIR /app

#copying package.json to the container
COPY package.json .

#install dependencies
RUN npm install

#expose port 5100 to the outside world
EXPOSE 5173

#copying rest of files to the container
COPY . .

#start application
CMD ["npm", "run","dev"]
