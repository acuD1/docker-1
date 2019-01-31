# 
# docker builde -t ft-rails .
# docker run --it -p 3000:3000 --rm ft-rails

FROM ruby

### Initialisation
RUN apt-get update && apt-get install -y nodejs
WORKDIR /opt/app
COPY /app /opt/app
RUN echo -e "FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]" > Dockerfile

### Configuration of Rails
RUN gem install rails bundler
RUN bundler install
RUN rails db:migrate
RUN rails db:seed
