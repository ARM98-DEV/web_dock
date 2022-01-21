FROM python:3.10-alpine

ENV APP_ROOT /src
ENV CONFIG_ROOT /config

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN mkdir ${CONFIG_ROOT}
COPY requirements.txt ${CONFIG_ROOT}/requirements.txt
RUN apk update && apk add postgresql-dev gcc python3-dev musl-dev
RUN apk add zlib-dev jpeg-dev
RUN /usr/local/bin/python -m pip install --upgrade pip
RUN pip install -r ${CONFIG_ROOT}/requirements.txt
RUN /usr/local/bin/python -m pip install --upgrade pip
RUN pip install -r ${CONFIG_ROOT}/requirements.txt
RUN /bin/sh -c python -m pip install Pillow

RUN mkdir ${APP_ROOT}

WORKDIR ${APP_ROOT}

RUN python -m pip install Pillow

CMD python manage.py makemigrations --noinput && \
    while ! python manage.py migrate --noinput > /dev/null 2>&1 ; do sleep 1 ; done && \
    python manage.py collectstatic --noinput && \
    python manage.py createsuperuser --user ahmadreza --email ahmadreza@outlook.com --noinput;
    	
COPY ./entrypoint.sh ${CONFIG_ROOT}/entrypoint.sh

RUN sed -i 's/\r$//g' ${CONFIG_ROOT}/entrypoint.sh


RUN chmod +x  ${CONFIG_ROOT}/entrypoint.sh


ADD . ${APP_ROOT}
RUN chmod -R 0777 /src/mysite/static
ENTRYPOINT ["/config/entrypoint.sh"]
