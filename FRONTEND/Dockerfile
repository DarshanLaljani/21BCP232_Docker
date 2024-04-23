FROM node:latest

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 8080

CMD [ "npm", "run", "dev","--","--host","0.0.0.0"]
