# docker build -t ccr.ccs.tencentyun.com/cube-studio/kubeflow-dashboard:2022.09.01 -f install/docker/Dockerfile .

FROM ccr.ccs.tencentyun.com/cube-studio/kubeflow-dashboard:base

RUN apt-get update && apt-get install -y -f mysql-client zip git

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash
RUN apt-get -y install nodejs && npm install yarn -g

RUN rm /usr/bin/python && ln -s /usr/bin/python3 /usr/bin/python && rm /usr/bin/pip && ln -s /usr/bin/pip3 /usr/bin/pip
RUN pip install pip --upgrade && pip install docstring-parser==0.14.1 kfp==1.8.0
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && chmod +x kubectl && mv kubectl /usr/bin/
RUN mkdir /kubeflow && wget -P /kubeflow/ https://docker-76009.sz.gfp.tencent-cloud.com/kubeflow/src.tar.gz && tar -zxvf /kubeflow/src.tar.gz -C /kubeflow/ && rm /kubeflow/src.tar.gz

RUN pip install kubernetes==18.20.0 click==6.7 jaeger-client==4.6.1 opentracing-instrumentation==3.3.1
COPY install/docker/docker-add-file/rest.py /usr/local/lib/python3.6/dist-packages/kubernetes/client/rest.py
COPY install/docker/docker-add-file/__init__.py /usr/local/lib/python3.6/dist-packages/marshmallow_enum/__init__.py

COPY myapp /home/myapp/myapp
COPY myapp/static/appbuilder/frontend /data/web/frontend
ENV PATH=/home/myapp/myapp/bin:$PATH
ENV PYTHONPATH=/home/myapp:/kubeflow/src/pipelines/sdk/python/:$PYTHONPATH

USER root
COPY install/docker/entrypoint.sh /entrypoint.sh

# ENTRYPOINT ["/entrypoint.sh"]
# HEALTHCHECK CMD ["curl", "-f", "http://localhost:80/health"]






