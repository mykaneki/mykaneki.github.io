```bash
sudo passwd
su root
mykaneki668CMY...

sudo apt-get install -y gcc make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev

cd /usr/local
mkdir /usr/local/python3
cd python3
wget https://www.python.org/ftp/python/3.10.10/Python-3.10.10.tgz
tar -xvf Python-3.10.10.tgz
cd Python-3.10.10
./configure --prefix=/usr/local/python3
make
make install
ln -s /usr/local/python3/bin/python3  /usr/bin/python3
ls -l /usr/bin/python3

```

