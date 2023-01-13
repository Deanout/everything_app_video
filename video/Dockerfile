# Make sure it matches the Ruby version in .ruby-version and Gemfile
ARG RUBY_VERSION=3.2.0
FROM ruby:$RUBY_VERSION

# Install packages need to build gems
RUN apt-get update -qq && \
    apt-get install -y build-essential libvips bash bash-completion libffi-dev tzdata postgresql nodejs npm yarn && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /usr/share/doc /usr/share/man

# Rails app lives here
WORKDIR /rails

# Set production environment
ENV RAILS_LOG_TO_STDOUT="1" \
    RAILS_SERVE_STATIC_FILES="true" \
    RAILS_ENV="production" \
    BUNDLE_WITHOUT="development" \
    POSTGRES_HOST=${POSTGRES_HOST} \
    POSTGRES_DB=${POSTGRES_DB} \
    POSTGRES_USER=${POSTGRES_USER} \
    POSTGRES_PASSWORD=${POSTGRES_PASSWORD} \
    RAILS_MASTER_KEY=${RAILS_MASTER_KEY} \
    RAILS_ENV=${RAILS_ENV} \
    VIRTUAL_HOST=${VIRTUAL_HOST} \
    VIRTUAL_PORT=${VIRTUAL_PORT} \
    LETSENCRYPT_HOST=${LETSENCRYPT_HOST} \
    LETSENCRYPT_EMAIL=${LETSENCRYPT_EMAIL}

# Install application gems
COPY Gemfile Gemfile.lock ./
RUN bundle install

# Copy application code
COPY . .

# Precompile bootsnap code for faster boot times
RUN bundle exec bootsnap precompile --gemfile app/ lib/

# Precompiling assets for production without requiring secret RAILS_MASTER_KEY
RUN SECRET_KEY_BASE_DUMMY=1 ./bin/rails assets:precompile

# Entrypoint prepares the database.
ENTRYPOINT ["/rails/bin/docker-entrypoint"]

# Start the server by default, this can be overwritten at runtime
EXPOSE 3000:3000
CMD ["./bin/rails", "server"]