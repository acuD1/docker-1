# Make sure to have TS3 client version 3.2.3
# docker build -t ts3-image .
# docker run -it --name ts3 -p 9987:9987/udp -p 10011:10011 -p 30033:30033 --rmi ts3-image

FROM debian:latest

### Env
ENV VERSION=3.6.1
ENV TS3SERVER_LICENSE=accept

### Initialisation
RUN apt-get update && apt-get upgrade -y
RUN apt-get install wget bzip2 -y

### Config
RUN wget http://dl.4players.de/ts/releases/${VERSION}/teamspeak3-server_linux_amd64-${VERSION}.tar.bz2
RUN tar -jxvf *.bz2

### Starting
ENTRYPOINT ["./teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh"]
