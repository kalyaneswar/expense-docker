FROM node:20.15.0-alpine3.20
EXPOSE 8080
ENV DB_HOST=mysql
# RUN useradd expense
# RUN mkdir /opt/server
# RUN chown expense:expense -R /opt/server
RUN addgroup -S expense && adduser -S expense -G expense \
    && mkdir /opt/server \
    && chown expense:expense -R /opt/server
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install
CMD [ "node","index.js" ]


# commands
# docker network
# docker network create expense
# ifconfig
# cat /etc/*release
# mysql -h mysql -u root -pExpenseApp@1 --> Debian

# Trobleshooting
# curl http://localhost:8080/transaction
