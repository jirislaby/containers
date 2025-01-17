# syntax=docker/dockerfile:1
FROM opensuse/tumbleweed
RUN zypper --non-interactive refresh
RUN zypper --non-interactive install \
  gcc \
  git-core \
  libyaml-devel \
  make \
  ruby-devel \
  sqlite3-devel

RUN useradd ruby
USER ruby
WORKDIR /home/ruby

COPY timestamp-clone .
RUN git clone https://github.com/jirislaby/clang-struct
WORKDIR clang-struct/frontend
COPY --chown=ruby:ruby structs.db storage/
COPY --chown=ruby:ruby credentials.yml.enc config/

RUN bundle config set --local path 'gem_home'
# with ruby 3.4, they don't have a binary sqlite3 yet, apparently...
# unset also force_ruby_platform: true in Gemfile and sqlite3-devel above
RUN bundle config build.sqlite3 --enable-system-libraries
RUN bundle install
#RUN bundle exec rails credentials:edit
COPY timestamp-update .
RUN git pull
RUN bundle exec rake assets:precompile

EXPOSE 3000
ENTRYPOINT bundle exec rails server -e production --no-log-to-stdout
