# syntax=docker/dockerfile:1
FROM opensuse/tumbleweed
RUN zypper ar -f obs://SUSE:CA/ suse-ca
RUN zypper ar -f obs://Kernel:tools/openSUSE_Factory k_tools
RUN zypper ar -f obs://home:jirislaby/ my_home
RUN zypper ar -f obs://home:jirislaby:file2config/ f2c
RUN zypper --non-interactive --gpg-auto-import-keys refresh
# make is needed for parallel gcc-lto
# java's build-classpath needs find (bsc#1245969)
RUN zypper --no-refresh --non-interactive install \
  antlr4 \
  bash-completion \
  ca-certificates-suse \
  ccache \
  find \
  gcc \
  gcc-c++ \
  git-core \
  libantlr4-runtime-devel \
  make \
  meson \
  ninja \
  patch \
  'pkgconfig(cxxopts)' \
  rapidquilt \
  slaby-scripts \
  slhelpers-devel \
  sqlite3 \
  vim \
  vim-data

RUN cat <<EOF > /etc/vimrc
so /usr/share/vim/current/suse.vimrc
so /usr/share/slaby-scripts/vimrc
EOF
RUN echo ". /usr/share/slaby-scripts/bashrc" >> /etc/bash.bashrc.local
RUN ln -s "/usr/share/slaby-scripts/bash_profile" /etc/profile.d/slaby.sh
COPY entrypoint /usr/bin/

RUN useradd -u 10043 jslaby
RUN mkdir /home/jslaby_loc
RUN chown jslaby /home/jslaby_loc
USER jslaby

RUN echo ".read /usr/share/slaby-scripts/sqliterc" > /home/jslaby/.sqliterc

WORKDIR /home/jslaby_loc
COPY timestamp .

USER 0
RUN zypper --non-interactive --gpg-auto-import-keys refresh
RUN zypper --no-refresh --non-interactive dup -l --no-recommends
USER jslaby

RUN git clone https://github.com/jirislaby/file2config
WORKDIR file2config
RUN meson setup build
RUN ninja -C build
ENV DESTDIR=/home/jslaby_loc/f2c-install/
RUN ninja -C build install

WORKDIR /src

ENV PATH=/home/jslaby_loc/f2c-install/usr/local/bin/:$PATH
ENV LINUX_GIT=/src/linux-dis

#USER 0
#RUN zypper --no-refresh --non-interactive install \
#USER jslaby

ENTRYPOINT [ "/usr/bin/entrypoint" ]
CMD [ "shell" ]
