FROM pennriver/ubuntu-xrdp:latest

RUN apt-get update && apt-get remove docker docker-engine docker.io
RUN apt-get update && apt-get install curl -y
RUN apt-get update && apt-get install apt-transport-https ca-certificates curl software-properties-common -y
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable edge"
RUN apt-cache policy docker-ce
RUN apt-get update && apt-get install docker-ce=18* -y

# Free Up GID 999
# export USR=$(id -nu 999) # UID => USERNAME
RUN export GRP=$(getent group 999 | cut -d: -f1) && groupmod -g 1999 ${GRP}

RUN apt-get update && apt-get install net-tools xorg openbox iputils-ping nmap -y
RUN apt-get update && apt-get install openssh-client gvfs-fuse gvfs-backends -y
RUN apt-get update && apt-get install gedit -y
RUN apt-get update && apt-get install file-roller -y
RUN apt-get update && apt-get install nano -y
RUN add-apt-repository -y ppa:tista/adapta && apt-get update && apt-get install adapta-gtk-theme -y
RUN apt-get update && apt-get install fonts-roboto fonts-noto -y
RUN add-apt-repository -y ppa:papirus/papirus && apt-get update && apt-get install papirus-icon-theme -y
RUN apt-get update && apt-get install tilix -y
RUN apt-get purge xscreensaver -y
RUN apt-get update && apt-get install xfce4-whiskermenu-plugin mugshot -y
RUN apt-get update && apt-get install meld -y
RUN apt-get update && apt-get upgrade -y
RUN apt-get update && apt-get install gawk -y
RUN apt-get remove xserver-xorg-legacy -y

# Docker Compose
RUN curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose && \
  chmod +x /usr/local/bin/docker-compose && \
  docker-compose --version

# Icons
COPY icons/scalable/ /usr/share/icons/hicolor/scalable/apps/
COPY icons/48x48/ /usr/share/icons/hicolor/48x48/apps/

# Scripts
RUN mkdir -p /tmp/scripts
COPY scripts/ /tmp/scripts/
RUN chmod +x /tmp/scripts/* && mv /tmp/scripts/* /usr/local/bin/

# Menu
COPY menu/ /usr/share/applications/

# Desktop
COPY config/xfce4/ /etc/xdg/xfce4/

CMD [ "start" ]

# BUILD SINGLE
# docker build -t local/rdp .

# RUN
# docker build -t local/rdp . ; docker stop local_rdp; docker rm local_rdp; docker run --name local_rdp --network oipa -p 3389:3389 local/rdp

# VANILLA
# docker stop local2_rdp; docker rm local2_rdp; docker run --name local2_rdp --network oipa -p 3389:3389 pennriver/ubuntu-xrdp:latest
