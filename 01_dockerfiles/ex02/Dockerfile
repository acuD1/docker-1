## Add any app compatible with ruby 2.5.1 and bundler 1.17.3, in my case I use a simple blog at https://github.com/acuD1/rails-docker-1.git
# $ git clone https://github.com/acuD1/rails-docker-1.git
# $ docker build -t ft-rails:on-build .
## Then run :
# $ mkdir ex02 && echo -e "FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]" > ex02/Dockerfile
## Then run subject build:
# $ docker build -t ex02 ex02/
# $ docker run -it -p 3000:3000 --rm ex02
FROM ruby:2.5.1

### Initialisation
RUN apt-get update && apt-get install -y nodejs
RUN gem install rails bundler

### Copy app to build
COPY . /opt/app
WORKDIR /opt/app

### Configuring app
ONBUILD RUN bundler install
ONBUILD RUN rails db:migrate
ONBUILD RUN rails db:seed
