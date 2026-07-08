FROM node:21

WORKDIR /app

COPY . .

RUN npm i

RUN apt-get update && apt-get install -y curl

COPY .env.sample .env

EXPOSE 5000

CMD ["npm", "start"]
