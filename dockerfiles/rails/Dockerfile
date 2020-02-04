FROM ruby:2.6-alpine

RUN uname -a
RUN echo 'http://mirror.leaseweb.com/alpine/latest-stable/main' > /etc/apk/repositories; \
    echo 'http://mirror.leaseweb.com/alpine/latest-stable/community' >> /etc/apk/repositories
RUN cat /etc/apk/repositories

RUN apk --no-cache upgrade && \
  apk add --no-cache \
  build-base \
  curl \
  git \
  libxml2-dev \
  libxslt-dev \
  nodejs-current \
  postgresql-dev \
  sqlite-dev \
  tzdata \
  yarn

RUN mkdir -p /home/geoblacklight/app
WORKDIR /home/geoblacklight/app

RUN git clone https://github.com/geoblacklight/workshop-app-2020.git .

RUN gem update bundler
RUN bundle check || bundle install --jobs "$(nproc)"
RUN yarn install

EXPOSE 3000
