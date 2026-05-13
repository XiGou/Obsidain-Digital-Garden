
## zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# plugins
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# ~/.zshrc
omz plugin enable zsh-autosuggestions zsh-syntax-highlighting


```

## Git
[[Git]]

## SSH

```bash
curl -s https://dufs.19960312.xyz/share/config/id_ed25519_gouxivae%40gmail.com.pub >> ~/.ssh/authorized_keys、


#~/.ssh/config

Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_gouxivae%40gmail.com
    IdentitiesOnly yes
```

## ninja
## cmake
## mold (replace ld)
[mold](https://github.com/rui314/mold)
## make
## global

## Powershell

```
$proxy="http://127.0.0.1:7891"

$env:http_proxy=$proxy
$env:https_proxy=$proxy
$env:HTTP_PROXY=$proxy
$env:HTTPS_PROXY=$proxy

curl https://github.com
```


```ps
# add path env
$env:PATH += ";C:\Users\JohnT\.local\bin"
```