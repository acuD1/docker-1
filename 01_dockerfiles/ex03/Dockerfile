# Setup memory Char VM : docker-machine create --driver virtualbox --virtualbox-memory "4096" Char

FROM ubuntu
WORKDIR ~/

# Initialisation
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y curl gnupg2 wget
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update
RUN apt-get install -y git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev nodejs yarn

# Installation of ruby 2.5.4 from source
RUN wget http://ftp.ruby-lang.org/pub/ruby/2.5/ruby-2.5.3.tar.gz
RUN tar -xzvf ruby-2.5.3.tar.gz
WORKDIR ~/ruby-2.5.3/
RUN ./configure
RUN make
RUN make install

# Configuring system for gitlab-ce
RUN gem install bundler -v 1.17.3
WORKDIR ~/
RUN wget https://dl.google.com/go/go1.11.5.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.11.5.linux_amd64.tar.gz
RUN export PATH=$PATH:/usr/local/go/bin
RUN source $HOME/.profile
