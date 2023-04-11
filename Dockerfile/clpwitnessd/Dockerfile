FROM node:latest

ARG version="5.1.0"

COPY clpwitnessd-${version}.tgz /tmp
RUN npm install --global /tmp/clpwitnessd-${version}.tgz

EXPOSE 8080
CMD ["clpwitnessd"]