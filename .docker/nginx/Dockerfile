FROM nginx:stable
LABEL Yann LUCAS <yann.lucas@outlook.com> for and adapted by Adrien PIERRARD <adrien.pierrard@gmail.com>

RUN apt-get update && apt-get install -y vim

ADD ./conf/.bashrc /root/.bashrc

RUN usermod -u 1000 www-data
