FROM alpine

RUN apk update
RUN apk add \
    vim \
    curl \
    python3 \
    py3-pip \
    consul \
    apache2

RUN python3 -m pip install supervisor

RUN mkdir -p /etc/consul/config/
RUN mkdir -p /etc/consul/data/
RUN mkdir -p /etc/supervisor/conf.d/

ADD ./consul-agent/client.json /etc/consul/config/client.json
ADD ./supervisor/confg.d/ /etc/supervisor/conf.d/
ADD ./supervisor/supervisord.conf /etc/supervisor/supervisord.conf

RUN echo "<center><h1><b>Httpd Container</b></h1></center" > /var/www/localhost/htdocs/index.html

CMD [ "supervisord", "-c", "/etc/supervisor/supervisord.conf" ]