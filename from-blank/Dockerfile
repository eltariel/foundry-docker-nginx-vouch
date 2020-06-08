FROM node:12-alpine

ENV UID=1000
ENV GID=1000

RUN deluser node
RUN adduser -u $UID -g $GID -D foundry

USER foundry
RUN mkdir -p /home/foundry/data
RUN mkdir -p /home/foundry/app

WORKDIR /home/foundry/app
COPY ./foundryvtt*.zip .
RUN unzip foundryvtt*.zip && rm foundryvtt*.zip

EXPOSE 30000
CMD ["node", "/home/foundry/app/resources/app/main.js", "--headless", "--dataPath=/home/foundry/data" ]
