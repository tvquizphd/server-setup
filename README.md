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
```
Configure tmux and vim settings:

```
echo "set-option -g prefix C-g" > ~/.tmux.conf
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

After that, copy the contents of `~/.ssh/id_rsa` into https://github.com/settings/keys.

Finally, clone the repository: 

```
ORGANIZATION=...
REPOSITORY=...
git clone git@github.com:$ORGANIZATION/$REPOSITORY.git
cd $REPOSITORY
git remote set-url origin tvquizphd:$ORGANIZATION/$REPOSITORY.git
```
