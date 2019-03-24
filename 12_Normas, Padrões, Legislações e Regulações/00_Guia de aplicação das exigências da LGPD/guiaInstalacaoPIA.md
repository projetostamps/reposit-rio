# Guia de Instalação - PIA

```
docker run --name pia --network host --volume "$(pwd)"/root:/root --volume "$(pwd)"/postgresql:/var/lib/postgresql -it ubuntu:17.10 /bin/bash

apt update && apt upgrade -y
apt install sudo net-tools build-essential vim git curl dirmngr ruby-full libpq-dev nodejs postgresql postgresql-client procps -y
service postgresql start
su - postgres

createuser --interactive -P

pia_development
p4ssw0rd
p4ssw0rd
y

[Ctrl][D]

gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
echo source /etc/profile.d/rvm.sh > ~/.bashrc

[Ctrl][D]

bash -c "clear && docker exec -it pia bash"

rvm install 2.5.3
rvm use 2.5.3 --default
gem install rails

cd ~
git clone https://github.com/LINCnil/pia-back.git
cd pia-back
cp config/database.example.yml config/database.yml
vi config/database.yml

=== config/database.yml ===
development:
  adapter:  postgresql
  host:     localhost
  encoding: unicode
  database: pia_development
  pool:     5
  username: pia_development
  password: p4ssw0rd
  template: template0
=== config/database.yml ===

gem update --system
gem install bundler
bundler update --bundler
bundle install

cp /root/pia-back/config/application.example.yml /root/pia-back/config/application.yml
echo "SECRET_KEY_BASE: $(rake secret)" | sudo tee /root/pia-back/config/application.yml
```
