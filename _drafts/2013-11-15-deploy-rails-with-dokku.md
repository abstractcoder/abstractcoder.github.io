---
title: Deploy Rails With Dokku
layout: post
---

Currently Dokku only support Ubuntu 13.04 x64.

Log into your VPS and run the following command. Edit your local /etc/hosts file and create an entry for you server.

example.com

```bash
wget -qO- https://raw.github.com/progrium/dokku/master/bootstrap.sh | sudo bash
```

```bash
cat ~/.ssh/id_rsa.pub | ssh progriumapp.com "sudo sshcommand acl-add dokku progrium"
```

Add gem 'rails_12factor' to Gemfile

cd /var/lib/dokku/plugins
git clone https://github.com/Kloadut/dokku-pg-plugin postgresql
dokku plugins-install

dokku postgresql:create [APP_NAME]

```bash
git remote add dokku dokku@progriumapp.com:node-js-app
git push progrium master
```

```bash
docker run -i -t app/name /bin/bash
export HOME=/app
for file in /app/.profile.d/*; do source $file; done
hash -r
cd /app
rake db:migrate
```

git remote add progrium dokku@progriumapp.com:node-js-app

dokku run www export HOME=/app; for file in /app/.profile.d/*; do source $file; done; hash -r; cd /app; bundle exec rake db:migrate

https://github.com/progrium/dokku/issues/156
https://gist.github.com/linjunpop/6247236