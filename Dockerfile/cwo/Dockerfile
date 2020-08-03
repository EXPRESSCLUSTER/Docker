FROM node:latest

RUN mkdir /opt/cwo
WORKDIR /opt/cwo

COPY cluster-webui-lin-4.2.2-200515-1.tgz .
COPY cluster-webui-sss-lin-4.2.2-200515-1.tgz .
COPY cluster-webui-sss-win-4.2.2-200515-1.tgz .
COPY cluster-webui-win-4.2.2-200515-1.tgz .

#RUN npm install --no-save cluster-webui-lin-4.2.2-200515-1.tgz ; exit 0
RUN npm install --no-save cluster-webui-lin-4.2.2-200515-1.tgz
RUN npm install --no-save cluster-webui-sss-lin-4.2.2-200515-1.tgz
RUN npm install --no-save cluster-webui-sss-win-4.2.2-200515-1.tgz
RUN npm install --no-save cluster-webui-win-4.2.2-200515-1.tgz

EXPOSE 29003
CMD ["/bin/bash","-c","cd /opt/cwo && /usr/local/bin/npx cluster-webui-win"]
