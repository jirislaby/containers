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
  libmariadb-devel \
  llvm-devel \
  make \
  mariadb-client \
  ncurses-devel \
  ninja \
  openssl-devel \
  'perl(DBD::MariaDB)' \
  'perl(DBI)' \
  'perl(Error)' \
  'perl(Git)' \
  'perl(JSON)' \
  'perl(JSON::XS)' \
  'perl(Parallel::ForkManager)' \
  slaby-scripts \
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

WORKDIR /home/jslaby_loc
COPY timestamp .
RUN git clone -b mariadb https://github.com/jirislaby/clang-struct
RUN mkdir clang-struct/build/
WORKDIR clang-struct/build/
RUN cmake -GNinja .. \
	-DCMAKE_C_COMPILER=clang \
	-DCMAKE_CXX_COMPILER=clang++ \
	-DCMAKE_BUILD_TYPE=RelWithDebInfo
RUN ninja -v
ENV DESTDIR=/home/jslaby_loc/clang-struct-install/
RUN ninja install

WORKDIR /src

ENV PATH=/home/jslaby_loc/clang-struct-install/usr/local/bin/:$PATH
ENV LD_LIBRARY_PATH=/home/jslaby_loc/clang-struct-install/usr/local/lib64/

#USER 0
#RUN zypper --no-refresh --non-interactive install \
#USER jslaby

ENTRYPOINT [ "/usr/bin/entrypoint" ]
CMD [ "all" ]
