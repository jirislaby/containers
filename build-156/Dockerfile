# syntax=docker/dockerfile:1
FROM registry.suse.com/bci/bci-base:15.6
RUN zypper ar -f obs://home:jirislaby/15.6 my_home
RUN zypper --non-interactive --gpg-auto-import-keys refresh
RUN zypper --no-refresh --non-interactive install \
  awk \
  bc \
  bison \
  ccache \
  diff \
  flex \
  gcc \
  git-core \
  libelf-devel \
  make \
  openssl-devel \
  pahole \
  python3

RUN zypper --no-refresh --non-interactive install \
  bash-completion \
  slaby-scripts \
  vim \
  vim-data

RUN cat <<EOF > /etc/vimrc
so /usr/share/vim/current/suse.vimrc
so /usr/share/slaby-scripts/vimrc
EOF
RUN echo ". /usr/share/slaby-scripts/bashrc" >> /etc/bash.bashrc.local
RUN ln -s "/usr/share/slaby-scripts/bash_profile" /etc/profile.d/slaby.sh

RUN useradd -u 10043 jslaby
USER jslaby
WORKDIR /src

ENTRYPOINT /bin/bash
