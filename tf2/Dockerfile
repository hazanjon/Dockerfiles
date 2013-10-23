# Team Fortress 2 Server
#
# Version 0.1

FROM ubuntu

MAINTAINER Jon Hazan <hazanjon@hotmail.com>

# make sure the package repository is up to date
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update

# Fake a fuse install
RUN apt-get install libfuse2
RUN cd /tmp ; apt-get download fuse
RUN cd /tmp ; dpkg-deb -x fuse_* .
RUN cd /tmp ; dpkg-deb -e fuse_*
RUN cd /tmp ; rm fuse_*.deb
RUN cd /tmp ; echo -en '#!/bin/bash\nexit 0\n' > DEBIAN/postinst
RUN cd /tmp ; dpkg-deb -b . /fuse.deb
RUN cd /tmp ; dpkg -i /fuse.deb

#Install 32-bit libs
RUN apt-get install -y ia32-libs

RUN apt-get install -y wget

#Install SteamPipe
RUN mkdir /usr/local/games/steam
RUN wget http://media.steampowered.com/client/steamcmd_linux.tar.gz
RUN tar -C /usr/local/games/steam -xvzf steamcmd_linux.tar.gz 
RUN /usr/local/games/steam/steamcmd.sh +login anonymous +force_install_dir /usr/local/games/steam/server +app_update 232250 validate +quit

#Fix for steam being daft
RUN mkdir /.steam
RUN ln -s /usr/local/games/steam/linux32 /.steam/sdk32

#Setup ports
EXPOSE 27015:27015
EXPOSE 27015:27015/udp
EXPOSE 27005:27005
EXPOSE 27005:27005/udp
EXPOSE 1200:1200
EXPOSE 1200:1200/udp

#CMD /bin/bash
ENTRYPOINT ["/usr/local/games/steam/server/srcds_run"]
CMD ["+sv_lan 0 -game tf -port 27015 +map plr_pipeline +maxplayers 24 -ip 0.0.0.0 -nobreakpad"]
