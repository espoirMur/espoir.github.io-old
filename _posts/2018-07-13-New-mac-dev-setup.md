

The first step is to install developer tools on the new laptop wihout installing Xcode (it's very large cannot afford to download it)

- Open the terminal
type the following one and follow the prompt :

```
xcode-select --install
```

follow [this link](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/) for a precise step by step guide .

The second step is to get rid of the default mac terminal  and install a powerfull and customizable terminal;
my choice is [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh).

let install it we need curl :

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
and now let customize it to make it a beautifull terminal !

default configuration can be edited and customized from the file .zshrc


```
nano ~/.zshr
```

open the file edit it according to you needs .
 you can pick some tips from [this guide](https://medium.com/@tretuna/macbook-pro-web-developer-setup-from-clean-slate-to-dev-machine-1befd4121ba8)
 
 once your terminal is now customized and fully functional we can continue istalling other tools
 
 - For customization i prefer using dracula theme for both vscode and iterm, it like a universal theme
 
 third step is to install package manager home brew:
 About homebrew:
 
 __Homebrew  a free and open-source software package management system 
 that simplifies the installation of software on Apple's macOS operating system__
 
 I will need this software to install most of the software I'm going to work with.
 
 From [their official site](https://brew.sh/)  it's said that :
 
__ Homebrew installs the stuff you need that Apple didn’t.__

that is the reason why it's not embedeed directly with ios like apt-get in linux or ubuntu os.

let us install it.
As homebrew is written in ruby we will install it with this simple comand:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
Once homebrew is installed we can install everything according to your needs.

For me as I'm a pyhton developer I will install everything related to python :

to start let install python3 and python 2


for python 2 i use 
```
brew install python@2
```

for python 3 I use 

```
brew install python
```


once python is installed I will install node and postgressql and docker

node and postgres can be installed via brew but for docker i will need to download a dmg directly.

The last software for my developement are text editor : i used atom and sublime text for developêment .
they can be downloaded directly from their site.
the next job will be to customise them for both python and javascript development
