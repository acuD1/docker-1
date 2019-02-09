# VM optimisation : docker-machine create --driver virtualbox --virtualbox-memory "4096" --virtualbox-cpu-count "2" Char
FROM ubuntu
WORKDIR /

# Initialisation
RUN apt-get update && apt-get upgrade -y
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y curl openssh-server ca-certificates postfix nodejs tzdata dirmngr gnupg git
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN apt-get install -y nodejs
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo 'root:qwerty' | chpasswd

# Gitlab-CE installation
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-get install gitlab-ce

# Unbundle nginx from Omnibus Gitlab
RUN echo "nginx['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "web_server['external_users'] = ['www-data']" >> /etc/gitlab/gitlab.rb
RUN echo "external_url \"https://192.168.99.100\"" >> /etc/gitlab/gitlab.rb
RUN echo "unicorn['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "gitlab_rails['internal_api_url'] = 'https://192.168.99.100'" >> /etc/gitlab/gitlab.rb

# Configure Nginx and Passenger
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
RUN apt-get install -y apt-transport-https ca-certificates
RUN sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger bionic main > /etc/apt/sources.list.d/passenger.list'
RUN apt-get update
RUN apt-get install -y --allow-unauthenticated nginx libnginx-mod-http-passenger
RUN if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi
RUN service nginx restart

# Installation of Ruby
RUN apt-get install ruby
RUN gem update --system
RUN gem install rubygems-update
RUN update_rubygems

# Configure HTTPS
RUN mkdir /var/log/gitlab/nginx
RUN mkdir /etc/ssl/dhparam
RUN openssl dhparam -dsaparam -out /etc/ssl/dhparam/dhparam.pem 4096
RUN openssl req -x509 -nodes -days 365 -newkey rsa:4096 \
		-subj /C=FR/ST=75/L=Paris/O=docker-1-gitlab/OU=docker-1-gitlab/CN=192.168.99.100/emailAddress=arsciand@student.42.fr \
		-keyout /etc/ssl/private/selfsigned.key -out /etc/ssl/certs/selfsigned.crt
RUN curl https://raw.githubusercontent.com/acuD1/Tools/master/gitlab.conf > /etc/nginx/sites-available/gitlab.conf
RUN ln -s /etc/nginx/sites-available/gitlab.conf /etc/nginx/sites-enabled/gitlab.conf
RUN nginx -t
RUN service nginx restart

ENTRYPOINT service ssh restart && service nginx restart && (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && bash
