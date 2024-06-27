# syntax=docker/dockerfile:1
FROM opensuse/tumbleweed
RUN zypper ar -f obs://home:jirislaby/ my_home
RUN zypper --non-interactive --gpg-auto-import-keys refresh
RUN zypper --no-refresh --non-interactive install \
  awk \
  bash-completion \
  bc \
  binutils \
  bison \
  ccache \
  clang \
  clang-devel \
  cmake \
  diff \
  findutils \
  flex \
  gcc \
  gcc-c++ \
  git-core \
  libelf-devel \
  llvm-devel \
  make \
  ncurses-devel \
  ninja \
  openssl-devel \
  'perl(DBD::SQLite)' \
  'perl(DBI)' \
  'perl(Error)' \
  'perl(Git)' \
  'perl(JSON)' \
  'perl(JSON::XS)' \
  'perl(Parallel::ForkManager)' \
  slaby-scripts \
  sqlite3 \
  sqlite3-devel \
  vim \
  vim-data

RUN cat <<EOF > /etc/vimrc
so /usr/share/vim/current/suse.vimrc
so /usr/share/slaby-scripts/vimrc
EOF
RUN echo ". /usr/share/slaby-scripts/bashrc" >> /etc/bash.bashrc.local
RUN ln -s "/usr/share/slaby-scripts/bash_profile" /etc/profile.d/slaby.sh

RUN useradd -u 10043 jslaby
RUN mkdir /home/jslaby_loc
RUN chown jslaby /home/jslaby_loc
USER jslaby

RUN echo ".read /usr/share/slaby-scripts/sqliterc" > /home/jslaby/.sqliterc

WORKDIR /home/jslaby_loc
COPY timestamp .
RUN git clone https://github.com/jirislaby/clang-struct
RUN mkdir clang-struct/build/
WORKDIR clang-struct/build/
RUN cmake -GNinja .. \
	-DCMAKE_C_COMPILER=clang \
	-DCMAKE_CXX_COMPILER=clang++ \
	-DCMAKE_BUILD_TYPE=RelWithDebInfo
RUN ninja
ENV DESTDIR=/home/jslaby_loc/clang-struct-install/
RUN ninja install

WORKDIR /src

ENV PATH /home/jslaby_loc/clang-struct-install/usr/local/bin/:$PATH
ENV LD_LIBRARY_PATH /home/jslaby_loc/clang-struct-install/usr/local/lib64/

#USER 0
#RUN zypper --no-refresh --non-interactive install \
#USER jslaby

ENTRYPOINT /bin/bash
