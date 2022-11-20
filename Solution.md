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
![https://raw.githubusercontent.com/TaanTV/TaanTV-test-task/main/id%20images.jpg](https://raw.githubusercontent.com/TaanTV/TaanTV-test-task/main/id%20images.jpg)

### при многочисленных попытках внутри контейнера подменить UID и GID приводят к такому выводу:

![https://raw.githubusercontent.com/TaanTV/TaanTV-test-task/main/user%20node%20is%20currently%20used%20by%20process%201.jpg](https://raw.githubusercontent.com/TaanTV/TaanTV-test-task/main/user%20node%20is%20currently%20used%20by%20process%201.jpg)

### Контейнер - это процесс. Процесс запущен пользователем и любая попытка подменить на горячую без изменений работающих родительских процессов от рута скорее всего не получится в работающем контейнере. Треб. уточенния. 

### я как правило пользуюсь практикой: 
1. Создаете новый образ с другим UID;
2. Пушите образ в репозиторий, обязательно меняя его номер версии;
3. Деплоите этот образ в Кубер (Кубер сам убьет старые версии и поднимет новые);
4. PROFIT!

### Повторюсь требует уточнения тех. задания.
--------------
#### мемасик для настроения
![https://raw.githubusercontent.com/TaanTV/TaanTV-test-task/main/mem.png](https://raw.githubusercontent.com/TaanTV/TaanTV-test-task/main/mem.png)