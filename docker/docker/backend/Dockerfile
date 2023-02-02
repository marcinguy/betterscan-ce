ARG DEBIAN_FRONTEND=noninteractive
#FROM ubuntu:20.04
FROM debian:11
RUN ln -snf /usr/share/zoneinfo/$CONTAINER_TIMEZONE /etc/localtime && echo $CONTAINER_TIMEZONE > /etc/timezone
RUN apt update && apt install -y libcurl4-nss-dev libssl-dev tree git sudo ssh rubygems python3-pip python3.9 python3.9-dev npm php default-jdk pipenv rsync jo libpq-dev curl
WORKDIR /
RUN mkdir -p /srv
RUN rm /usr/bin/python3
RUN ln -s /usr/bin/python3.9 /usr/bin/python3
RUN apt update && apt install -y git
RUN git clone -b python_3_9 https://github.com/marcinguy/scanmycode-ce.git /srv/scanmycode
WORKDIR /srv/scanmycode
RUN git pull
RUN apt update && apt install -y libcurl4-nss-dev libssl-dev tree sudo git ssh rsync npm ruby-sass pipenv
RUN tree
#RUN pip install -r requirements.txt
#RUN pipenv install --python=/usr/bin/python3.9
RUN python3 -m pipenv install --python=/usr/bin/python3.9 --system
RUN python3.9 -m pip install pylint===1.9.2
RUN python3.9 -m pip install stripe
WORKDIR /
RUN git clone -b checkmate3  https://github.com/marcinguy/checkmate-ce /checkmate
WORKDIR /checkmate
RUN tree /checkmate
RUN python3.9 setup.py install
WORKDIR /
#RUN pip uninstall -y blitzdb3
#RUN git clone https://github.com/marcinguy/blitzdb3-ce.git /blitzdb3-ce
#WORKDIR /blitzdb3-ce
#RUN git pull
#RUN python3 setup.py install
WORKDIR /
RUN ln -s /srv/scanmycode/quantifiedcode/settings/default.yml /srv/scanmycode/settings.yml
RUN python3.9 -m pip install psycopg2 --upgrade
#WORKDIR /
#RUN wget https://nodejs.org/dist/v16.14.0/node-v16.14.0-linux-x64.tar.xz
#RUN mkdir -p /usr/local/lib/nodejs
#RUN tar -xJvf node-v16.14.0-linux-x64.tar.xz -C /usr/local/lib/nodejs 
#ENV PATH "/usr/local/lib/nodejs/node-v16.14.0-linux-x64/bin:${PATH}"
WORKDIR /srv/scanmycode/quantifiedcode/frontend
RUN curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
RUN apt -y install nodejs
RUN npm install
RUN chmod -R ugo+rw /srv/scanmycode/quantifiedcode/frontend
WORKDIR /srv/scanmycode/quantifiedcode/frontend
RUN useradd user
RUN mkdir /home/user
RUN chmod -R oug+rwx /home/user
RUN sudo -u user make
WORKDIR /srv/scanmycode/quantifiedcode/plugins/git
RUN chmod -R ugo+rw /srv/scanmycode/quantifiedcode/plugins/git 
WORKDIR /srv/scanmycode/quantifiedcode/plugins/git/frontend
RUN sudo -u user make
RUN python3.9 -m pip install email_validator
#RUN pip3 install celery==4.4.6
RUN python3.9 -m pip install sqlalchemy===1.4.46
RUN curl --silent --output /dev/null https://dl.betterscan.io/docker-build-backend.html
