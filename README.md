# docker-gitlab
gitlab-ce as a docker container without a db and a proxy

These dockerfiles create a docker image with a gitlab-ce server without database and web (proxy) servers inside.
See the schema.png for details.

Volumes that can be mapped from host to container:
- git repository
- gitlab config
- gitlab log
- gitlab assets
- supervisord.conf
- supervisor log

Final image is created in two steps - before creating config files and after.

How to install.
1. Prepare the database.
2. Create the first image.
3. Run interactive container to copy configuration files to host folder.
4. Copy and rename example configuration files and set settings in them:
  - gitlab.yml
  - database.yml
  - secrets.yml
  - resque.yml
  - unicorn.rb
  - rack_attack.rb
  - smtp_settings.rb
5. Run interactive container to initialize the database.
6. Copy created config files near the second docker file to be injected into the second image.
7. Create the second image.
8. Run interactive container to copy assets to the host folder.
9. Create webserver (proxy) configuration.
10. Create resulting daemon container.
