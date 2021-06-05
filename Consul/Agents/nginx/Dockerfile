FROM alpine

RUN apk update
RUN apk add \
    vim \
    curl \
    python3 \
    py3-pip \
    consul \
    nginx

RUN python3 -m pip install supervisor

RUN mkdir -p /etc/consul/config/
RUN mkdir -p /etc/consul/data/
RUN mkdir -p /etc/supervisor/conf.d/
RUN mkdir /run/nginx/
RUN mkdir /var/www/html/

ADD ./consul-agent/client.json /etc/consul/config/client.json
ADD ./supervisor/confg.d/ /etc/supervisor/conf.d/
ADD ./supervisor/supervisord.conf /etc/supervisor/supervisord.conf
ADD ./nginx-config/default.conf /etc/nginx/conf.d/default.conf

RUN echo "<center><h1><b>Nginx Container</b></h1></center" > /var/www/html/index.html

CMD [ "supervisord", "-c", "/etc/supervisor/supervisord.conf" ]