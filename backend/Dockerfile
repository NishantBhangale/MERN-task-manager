FROM node:24-alpine

WORKDIR /app

COPY package*.json /app/

RUN npm ci 

COPY . .

EXPOSE 5000

CMD ["npm","start"]