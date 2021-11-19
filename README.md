These instructions are for Debian or Ubuntu.

## Set up server for development

```
apt-get install git -y
apt-get install tmux -y
apt-get install mosh -y
apt-get install firewalld -y
```
Configure git:
```
git config --global user.email "tvquizphd@gmail.com"
git config --global user.name "TVQuizPhD"
git config --global core.editor "vim"
```
Configure tmux and vim settings:

```
echo "set-option -g prefix C-t" > ~/.tmux.conf
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
wget -O ~/.vimrc https://gist.githubusercontent.com/tvquizphd/2b3632cf7c12d62c32d84c8ce1656940/raw/abf667c06cf4a56fe77617019efb80bf2a117e9f/.vimrc
vim -c "PluginInstall"
```

Configure mosh firewall settings:

```
firewall-cmd --zone=public --permanent --add-port=60000-61000/udp
firewall-cmd --reload
```

Configure SSH github settings:

```
mkdir ~/.ssh
wget -O ~/.ssh/config https://gist.githubusercontent.com/tvquizphd/2b3632cf7c12d62c32d84c8ce1656940/raw/af2113ef824e2416a21bef8b7554efc3b6651217/ssh.config
```

Create an SSH key:

```
ssh-keygen -t rsa -b 4096 -C "tvquizphd@gmail.com"
```

After that, copy the contents of `~/.ssh/id_rsa.pub` into https://github.com/settings/keys.

Finally, clone the repository: 

```
ORGANIZATION=...
REPOSITORY=...
git clone tvquizphd:$ORGANIZATION/$REPOSITORY.git
cd $REPOSITORY
```

### Optional steps

In cases where memory is a limiting factor, add some swap space:

```
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

In cases where storage is a limiting factor, reduce log size:

Set `SystemMaxUse=50M` in `/etc/systemd/journald.conf`.
Then run `journalctl --vacuum-size=50M` to clear excess logs.

Remove any large cached files found with `du -ahd1 ~/.cache/`.

Run `yarn cache clean` to clean yarn cache.
