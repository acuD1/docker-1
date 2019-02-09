# VM optimisation : docker-machine create --driver virtualbox --virtualbox-memory "4096" --virtualbox-cpu-count "2" Char]
FROM debian
WORKDIR /

# Initialisation
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y curl openssh-server ca-certificates postfix
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo 'root:qwerty' | chpasswd

# Gitlab-CE installation
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-get install gitlab-ce

# Configure HTTPS
RUN mkdir -p /etc/gitlab/ssl
RUN chmod 700 /etc/gitlab/ssl
RUN openssl req -x509 -nodes -days 365 -newkey rsa:4096 \
	-subj /C=FR/ST=75/L=Paris/O=rs1/OU=rs1/CN=192.168.99.100/emailAddress=arsciand@student.42.fr \
	-keyout /etc/gitlab/ssl/selfsigned.key -out /etc/gitlab/ssl/selfsigned.crt

RUN echo "external_url \"https://192.168.99.100\"" >> /etc/gitlab/gitlab.rb
RUN echo "nginx['redirect_http_to_https'] = true" >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/selfsigned.crt\"" >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/selfsigned.key\"" >> /etc/gitlab/gitlab.rb

ENTRYPOINT service ssh start && (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && bash

# When git clone or git push with HTTPS add 'GIT_SSL_NO_VERIFY=true' at the begining of each commands like '$ GIT_SSL_NO_VERIFY=true git push'
# When git clone with https://* in directory do '$ git config http.emptyAuth true' to avoid HTTP error
