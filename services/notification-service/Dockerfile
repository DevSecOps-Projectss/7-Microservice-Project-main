# syntax=docker/dockerfile:1

# ============================
# Stage 1 - Build dependencies
# ============================

FROM ruby:3.3-alpine AS build

WORKDIR /app

RUN apk add --no-cache \
    build-base \
    postgresql-dev

COPY Gemfile Gemfile.lock ./

RUN gem install bundler -v 4.0.20 --no-document && \
    bundle config set path /usr/local/bundle && \
    bundle install


# ============================
# Stage 2 - Runtime
# ============================

FROM ruby:3.3-alpine AS runtime

WORKDIR /app

RUN apk add --no-cache libpq && \
    addgroup -S appgroup && \
    adduser -S appuser -G appgroup

ENV GEM_HOME=/usr/local/bundle
ENV BUNDLE_PATH=/usr/local/bundle
ENV PATH="/usr/local/bundle/bin:${PATH}"

COPY --from=build \
    /usr/local/bundle \
    /usr/local/bundle

COPY --chown=appuser:appgroup \
    Gemfile \
    Gemfile.lock \
    app.rb \
    ./

USER appuser

EXPOSE 8086

CMD ["bundle", "exec", "ruby", "app.rb"]
