
## Install

```bash
wget https://ftp.gnu.org/pub/gnu/global/global-6.6.12.tar.gz
```

Unzip it `tar -zxvf`,  Then Read `INSTALL`.

## Usage

```bash
# gen DB
gtags -v

# update db
global -u

```

##  Ignore Some Dirs
`cp /usr/local/share/gtags/gtags.conf ~/.globalrc`  
Edit ":skip=", ref [emacs - how gtags exclude some specific subdirectories - Stack Overflow](https://stackoverflow.com/questions/42315741/how-gtags-exclude-some-specific-subdirectories)