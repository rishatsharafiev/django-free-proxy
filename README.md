# django-free-proxy

### Pre-install configuration
```
# docker
sudo apt-get update
sudo apt-get install htop python-pip -y

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   edge"
sudo apt-get update
sudo apt-get install docker-ce -y



# docker compose
sudo pip install docker-compose

# enable swap 2G
sudo swapoff /swapfile

sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile

sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapon --show

sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

sudo nano /etc/sysctl.conf
vm.swappiness=10 # add to /etc/sysctl.conf
vm.vfs_cache_pressure = 50 # add to /etc/sysctl.conf


sudo sysctl vm.swappiness=10
sudo sysctl vm.vfs_cache_pressure=50

# create user 
mkdir /home/django-free-proxy
useradd -d /home/django-free-proxy django-free-proxy
passwd django-free-proxy # add password to user
chown django-free-proxy:django-free-proxy /home/django-free-proxy -R
nano /etc/sudoers
django-free-proxy  ALL=(ALL) ALL
 # add to sudo 
nano /etc/sudoers

# docker without sudo
su django-free-proxy
sudo groupadd docker
sudo usermod -aG docker $USER

# ufw
sudo apt install ufw
sudo nano /etc/default/ufw 
IPV6=yes # in  /etc/default/ufw

sudo ufw default deny incoming
sudo ufw default allow outgoing

sudo ufw allow ssh
sudo ufw allow 22
sudo ufw allow 2222

sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 54321/tcp
sudo ufw allow 9001:9003/tcp
sudo ufw allow 9010/tcp
```

### Docker Compose build
```
rm celeryev.pid
cd devops && docker-compose -p django-free-proxy build
```

### Docker Compose up
```
docker-compose -p django-free-proxy up --force-recreate -d
```

### Connect to docker
```
docker exec -it django-free-proxy_app_1 sh
```

### Static and media
```
sudo chmod 777 static -R
sudo chmod 777 media -R
```

### Install Supervisor
```
apt-get install supervisor
```
