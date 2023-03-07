ARG UBUNTU_VERSION
FROM ubuntu:${UBUNTU_VERSION}

LABEL maintainer="guillaume.chauveau@appus.fr"

RUN apt-get update -y \
    && apt-get install -y python-pip python-dev \
    && pip install --upgrade setuptools \
    && pip install mysql-connector-python pandas xlwt xlrd flask

CMD ["tail", "-F", "-n0" ,"/etc/hosts"]