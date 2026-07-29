apt update
apt install -y xfce4 xfce4-goodies xorg dbus-x11

apt install -y xrdp
systemctl enable --now xrdp

wget https://github.com/xpf0000/FlyEnv/releases/download/v4.17.2/FlyEnv-4.17.2-x64.deb

sudo apt update

sudo apt install ./FlyEnv-4.17.2-x64.deb

adduser developer
usermod -aG sudo developer
