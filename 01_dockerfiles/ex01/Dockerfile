# docker build -t ts3 .
# docker run -it ts3 -p 9987:9987/udp -p 10011:10011 -p 30033:30033 --rm

FROM debian:latest

### Initialisation
RUN apt-get update && apt-get upgrade -y
RUN apt-get install wget bzip2 -y

### Ports
EXPOSE 9987/udp
EXPOSE 10011
EXPOSE 30033

### Config
run wget https://files.teamspeak-services.com/releases/server/3.6.0/teamspeak3-server_linux_amd64-3.6.0.tar.bz2
RUN tar -jxvf *.bz2

### Starting
ENV TS3SERVER_LICENSE accept
ENTRYPOINT ["./teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh"]
