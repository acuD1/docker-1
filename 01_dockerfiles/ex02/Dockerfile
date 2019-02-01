## Add any app compatible with ruby 2.5.1 and bundler 1.17.3, in my case I use a simple blog at https://github.com/acuD1/rails-docker-1-app.git
# $ git clone https://github.com/acuD1/rails-docker-1-app.git app
# $ docker build -t ft-rails:on-build .
## Create the Dockerfile from the subject
# $ echo -e 'FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]' > Dockerfile_Subject
## Then run subject build:
# $ docker build -t ex02 -f Dockerfile_Subject .
# $ docker run -it -p 3000:3000 --rm ex02
FROM ruby:2.5.1

### Initialisation
RUN apt-get update && apt-get install -y nodejs
RUN gem install rails bundler

### Copy app to build
COPY app /opt/app
WORKDIR /opt/app

### Configuring app
ONBUILD RUN bundler install
ONBUILD RUN rails db:migrate
ONBUILD RUN rails db:seed
