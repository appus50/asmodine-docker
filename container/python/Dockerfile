ARG UBUNTU_VERSION
FROM ubuntu:${UBUNTU_VERSION}

LABEL maintainer="guillaume.chauveau@appus.fr"

RUN apt-get update -y \
    && apt-get install -y python3 python3-pip python3-dev \
    && pip3 install --upgrade setuptools \
    && pip3 install python-dotenv Sphinx sphinx_rtd_theme numpy matplotlib pandas seaborn sklearn xgboost jupyter xlrd flask pyhocon joblib tqdm elasticsearch pika mysql-connector-python pyyaml

CMD ["tail", "-F", "-n0" ,"/etc/hosts"]
