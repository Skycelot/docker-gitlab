FROM skycelot/gitlab-preconfig

LABEL maintainer "Mikhail Avakov <mikleavakov@gmail.com>"

COPY gitlab.yml /home/git/gitlab/config/
COPY unicorn.rb /home/git/gitlab/config/
COPY database.yml /home/git/gitlab/config/
COPY secrets.yml /home/git/gitlab/config/
COPY smtp_settings.rb /home/git/gitlab/config/initializers/
COPY resque.yml /home/git/gitlab/config/
COPY rack_attack.rb /home/git/gitlab/config/initializers/

RUN bundle exec rake gitlab:shell:install REDIS_URL=unix:/var/run/redis/redis.sock RAILS_ENV=production SKIP_STORAGE_VALIDATION=true && \
    bundle exec rake "gitlab:workhorse:install[/home/git/gitlab-workhorse]" RAILS_ENV=production && \
    npm install --production && \
    bundle exec rake gitlab:assets:compile RAILS_ENV=production NODE_ENV=production

USER root

COPY supervisord.conf /etc/

CMD ["/usr/bin/supervisord"]
