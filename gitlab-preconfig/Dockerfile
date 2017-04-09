FROM ubuntu:xenial

LABEL maintainer "Mikhail Avakov <mikleavakov@gmail.com>"

WORKDIR /tmp/gitlab-dependencies

RUN apt-get update && \
    apt-get install -y \
      build-essential \
      zlib1g-dev \
      libz-dev \
      libyaml-dev \
      libssl-dev \
      libgdbm-dev \
      libreadline-dev \
      libncurses5-dev \
      libffi-dev \
      libexpat1-dev \
      libpq-dev \
      gettext \
      curl \
      apt-transport-https \
      openssh-server \
      redis-server \
      checkinstall \
      libxml2-dev \
      libxslt-dev \
      libcurl4-openssl-dev \
      libicu-dev \
      logrotate \
      python-docutils \
      pkg-config \
      cmake \
      supervisor \
      lsof \
      mc && \
    wget -q https://www.kernel.org/pub/software/scm/git/git-2.12.0.tar.gz && \
    tar xzf git-2.12.0.tar.gz && \
    cd git-2.12.0 && \
    ./configure && \
    make && \
    make install && \
    cd .. && \
    wget -q https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.3.tar.gz && \
    tar xzf ruby-2.3.3.tar.gz && \
    cd ruby-2.3.3 && \
    ./configure --disable-install-rdoc && \
    make && \
    make install && \
    gem install bundler --no-ri --no-rdoc && \
    cd .. && \
    wget -q https://storage.googleapis.com/golang/go1.5.3.linux-amd64.tar.gz && \
    tar xzf go1.5.3.linux-amd64.tar.gz -C /usr/local && \
    ln -s /usr/local/go/bin/go /usr/local/bin && \
    ln -s /usr/local/go/bin/godoc /usr/local/bin && \
    ln -s /usr/local/go/bin/gofmt /usr/local/bin && \
    curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
    echo "deb https://deb.nodesource.com/node_6.x xenial main" > /etc/apt/sources.list.d/nodejs.list && \
    apt-get update && \
    apt-get install -y nodejs && \
    adduser --disabled-login --gecos 'Gitlab' --uid 850 git && \
    usermod -aG redis git

COPY redis.conf /etc/redis/

RUN mkdir /var/run/redis && \
    chown redis:redis /var/run/redis && \
    chown redis:redis /etc/redis/redis.conf && \
    chmod o+r /etc/redis/redis.conf

WORKDIR /home/git

RUN rm -rf /tmp/gitlab-dependencies

USER git

RUN git clone https://gitlab.com/gitlab-org/gitlab-ce.git -b 8-17-stable gitlab

WORKDIR /home/git/gitlab

RUN bundle install --deployment --without development test mysql aws kerberos && \
    git config --global gc.auto 0 && \
    git config --global core.autocrlf input && \
    git config --global repack.writeBitmaps true && \
    mkdir /home/git/gitlab/public/uploads

CMD ["/bin/bash"]
