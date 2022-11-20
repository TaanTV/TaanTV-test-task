# Выполнение тех. задания

### Dockerhub

https://hub.docker.com/r/dmitrykraska/test-task-grguta/tags


### Dockerfile

````
FROM python:3.8

COPY . /app

ARG UID=2000
ARG GID=2000
ARG USER=node

ENV UID=${UID}
ENV GID=${GID}
ENV USER=${USER}

RUN groupadd --gid ${GID} node \
  && useradd --uid ${UID} --gid node --shell /bin/bash --create-home node

RUN chown -R $USER:$USER /app
RUN cd /app && pip install -e .

USER ${USER}

CMD pgcli
````

### результат

![]

