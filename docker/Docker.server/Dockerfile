FROM python:3.7.3

ENV PYTHONUNBUFFERED 1

RUN apt-get update && apt-get install -y \
    sudo && apt-get install -y \
    git \
    nginx \
    postgresql-client \
    python-psycopg2 \
    supervisor
RUN pip install --upgrade pip
RUN python3 -m pip install gunicorn

RUN mkdir /eathis
WORKDIR /eathis/server

RUN rm /etc/supervisor/supervisord.conf && \
    ln -s /eathis/docker/Docker.server/supervisord.conf /etc/supervisor/supervisord.conf && \
    rm /etc/nginx/nginx.conf && \
    ln -s /eathis/docker/Docker.server/nginx.conf /etc/nginx/nginx.conf && \
    mkdir -p /var/log/supervisor && \
    mkdir -p /var/log/gunicorn && \
    mkdir -p /eathis/logs/supervisord

ADD server /eathis
ADD .env /eathis
ADD docker /eathis

# Install python requirements
ADD requirements.txt /eathis/
RUN pip install -r /eathis/requirements.txt

RUN groupadd -r app -g 1000 && \
    useradd -u 1000 -r -g app -d /eathis -s /bin/bash -c "Docker image user" app
RUN chown -R app:app /eathis && \
    chown -R app:app /var/run && \
    chown -R app:app /var/log/gunicorn

EXPOSE 5000

ENTRYPOINT ["/bin/bash", "/eathis/docker/Docker.server/entrypoint.sh"]
CMD ["development"]
