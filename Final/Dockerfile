FROM ruby:3.2.2-alpine
LABEL maintainer="krmbzds.github@gmail.com"

# Add basic packages
RUN apk add --no-cache \
      xz-libs \
      postgresql-client \
      gcompat \
      brotli-libs \
      tzdata \
      file

# Configure Rails
ENV RAILS_LOG_TO_STDOUT true
ENV RAILS_SERVE_STATIC_FILES true
ENV MALLOC_ARENA_MAX 2

WORKDIR /app

# Expose Puma port
EXPOSE 3000

# This image is for development env only
ENV RAILS_ENV development

# Write GIT meta data from arguments to env vars
ONBUILD ARG COMMIT_SHA
ONBUILD ARG COMMIT_TIME
ONBUILD ARG COMMIT_VERSION

ONBUILD ENV COMMIT_SHA ${COMMIT_SHA}
ONBUILD ENV COMMIT_TIME ${COMMIT_TIME}
ONBUILD ENV COMMIT_VERSION ${COMMIT_VERSION}

# Add user
ONBUILD RUN addgroup -g 1000 -S app && \
            adduser -u 1000 -S app -G app

# Copy app with gems from former build stage
ONBUILD COPY --from=Builder --chown=app:app /usr/local/bundle/ /usr/local/bundle/
ONBUILD COPY --from=Builder --chown=app:app /app /app
