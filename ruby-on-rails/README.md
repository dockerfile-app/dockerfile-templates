# About Ruby on Rails

<a target="_blank" rel="noopener noreferrer" href="https://rubyonrails.org/">
    <img alt="Ruby on Rails Logo" align="right" src="/ruby-on-rails/logo.png" width="100px" />
</a>

[Ruby on Rails](https://rubyonrails.org/), or Rails, is a server-side web application framework written in Ruby under the MIT License. Rails is a model–view–controller framework, providing default structures for a database, a web service, and web pages.

## Sample Dockerfile

```Dockerfile
FROM ruby:3.0.0-alpine AS Builder

ARG RAILS_ROOT=/app
ARG BUILD_PACKAGES="build-base curl-dev git"
ARG DEV_PACKAGES="sqlite-dev yaml-dev zlib-dev nodejs yarn"
ARG RUBY_PACKAGES="tzdata"
ARG SECRET_KEY_BASE

ENV BUNDLE_APP_CONFIG="$RAILS_ROOT/.bundle"
WORKDIR $RAILS_ROOT

# Install packages
RUN apk update \
    && apk upgrade \
    && apk add --update --no-cache $BUILD_PACKAGES $DEV_PACKAGES $RUBY_PACKAGES
COPY Gemfile* package.json yarn.lock ./

# Install rubygem
COPY Gemfile Gemfile.lock $RAILS_ROOT/
RUN bundle config --global frozen 1 \
    && bundle install --without development:test:assets -j4 --retry 3 --path=vendor/bundle \
    # Remove unneeded files (cached *.gem, *.o, *.c)
    && rm -rf vendor/bundle/ruby/3.0.0/cache/*.gem \
    && find vendor/bundle/ruby/3.0.0/gems/ -name "*.c" -delete \
    && find vendor/bundle/ruby/3.0.0/gems/ -name "*.o" -delete
    
RUN yarn install
COPY . .

RUN bin/rails webpacker:compile
RUN bin/rails assets:precompile

# Remove folders not needed in resulting image
RUN rm -rf node_modules tmp/cache vendor/assets spec

# Build step complete.


FROM ruby:3.0.0-alpine
ARG RAILS_ROOT=/app
ARG PACKAGES="tzdata sqlite sqlite-dev nodejs bash"
ENV BUNDLE_APP_CONFIG="$RAILS_ROOT/.bundle"
WORKDIR $RAILS_ROOT

# install packages
RUN apk update \
    && apk upgrade \
    && apk add --update --no-cache $PACKAGES

# Add user
RUN addgroup -g 1000 -S app \
 && adduser -u 1000 -S app -G app
USER app

COPY --from=Builder --chown=app:app $RAILS_ROOT $RAILS_ROOT

EXPOSE 3000
CMD ["bin/rails", "server", "-b", "0.0.0.0"]
```
