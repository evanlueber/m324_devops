FROM node:latest
WORKDIR /app
RUN git clone https://github.com/evanlueber/m324_devops.git
WORKDIR /app/m324_devops/aufgabe1
EXPOSE 3000
ENTRYPOINT ["npm", "run", "start"]
