# syntax=docker/dockerfile:1
FROM opensuse/tumbleweed
RUN zypper ar -f obs://home:jirislaby/ my_home
RUN zypper --non-interactive --gpg-auto-import-keys refresh
RUN zypper --no-refresh --non-interactive install \
  bash-completion \
  colordiff \
  findutils \
  git-core \
  less \
  osc \
  postgresql-server \
  'python3dist(file-magic)' \
  'python3dist(osc)' \
  'python3dist(pyyaml)' \
  'python3dist(psycopg)' \
  'python3dist(requests)'  \
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
RUN mkdir /run/postgresql
RUN chown jslaby /run/postgresql
USER jslaby

WORKDIR /home/jslaby_loc
COPY timestamp .
RUN git clone https://src.opensuse.org/importers/git-importer.git
WORKDIR git-importer

#USER 0
#RUN zypper --no-refresh --non-interactive install \
#USER jslaby

ENTRYPOINT [ "/usr/bin/entrypoint" ]
CMD [ "shell" ]
