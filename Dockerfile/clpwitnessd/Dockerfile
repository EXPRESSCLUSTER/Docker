FROM node:latest

COPY clpwitnessd-4.2.0.tgz /tmp
RUN npm install --global /tmp/clpwitnessd-4.2.0.tgz

EXPOSE 80
CMD ["clpwitnessd"]
