#base image
FROM node:18.9.1

#set work directory
WORKDIR /app

COPY package.json .

#dependencies installatiom
RUN npm install 

COPY . .

#expose port
EXPOSE 5050

#start command

CMD ["npm", "start"]


