FROM node:latest

ARG version="5.1.0-221226-1"

RUN mkdir /opt/cwo
WORKDIR /opt/cwo

COPY cluster-webui-lin-${version}.tgz .
COPY cluster-webui-sss-lin-${version}.tgz .
COPY cluster-webui-sss-win-${version}.tgz .
COPY cluster-webui-win-${version}.tgz .

RUN npm install --no-save cluster-webui-lin-${version}.tgz
RUN npm install --no-save cluster-webui-sss-lin-${version}.tgz
RUN npm install --no-save cluster-webui-sss-win-${version}.tgz
RUN npm install --no-save cluster-webui-win-${version}.tgz

EXPOSE 29003
CMD ["/bin/bash","-c","cd /opt/cwo && /usr/local/bin/npx cluster-webui-win"]