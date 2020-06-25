# Centos Software Installation

## zsh & oh my zsh

1.Install the zsh package

```shell
yum -y installl zsh
```

2.Switch the default shell to zsh

```shell
chsh -s /bin/zsh
```

3.Install on my zsh

curl

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

wget

```shell
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)
```
