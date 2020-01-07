This guide helps to setup a new macbook and make it ready for software development.

## Make it yours : (Change the Laptop Name):

I use the following command to change the macbook name in case it's promping a wrong username:

```sudo scutil --set HostName name-you-want```


## Developers Tools 

### Xcode 

The first step is to install developer tools on the new laptop wihout installing Xcode (it's very large cannot afford to download it)

- Open the terminal
- type the following one and follow the prompt :

```
xcode-select --install
```

follow [this link](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/) for a precise step by step guide .

### Terminal

The second step is to get rid of the default mac terminal  and install a powerfull and customizable terminal;
my choice is  a combinaison of zsh and [iterm2](https://www.iterm2.com/documentation.html), for [zsh](https://www.zsh.org/) customization I use [oh-my-zsh](https://ohmyz.sh/)

let install it we need curl :

- For iterm2
``` brew cask install iterm2 ```

- For Zsh  and zsh completions
``` brew install zsh zsh-completions  ```

- And oh my zsh c: 
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

And now we can customize it as we need 

The default configuration can be edited and customized from the file .zshrc
More about the terminal configuration can be found [here](https://dev.to/deepu105/configure-a-beautiful-terminal-on-unix-with-zsh-4mcb).

 - For customization i prefer using dracula theme for both vscode and iterm, it like a universal theme
 
### Package Manager Homebrew

Third step is to install package manager homebrew:
About homebrew:
 
 _Homebrew  a free and open-source software package management system 
 that simplifies the installation of software on Apple's macOS operating system_
 
 I will need this software to install most of the software I'm going to work with.
 
 From [their official site](https://brew.sh/)  it's said that :
 
__Homebrew installs the stuff you need that Apple didn’t._

that is the reason why it's not embedeed directly with ios like apt-get in linux or ubuntu os.

let us install it.
As homebrew is written in ruby we will install it with this simple comand:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
Once homebrew is installed we can install everything according to your needs.

## Useful Softwares

I run the following comand to install the first set of softwares I need to work on my new laptop:

```
 brew cask install slack zoomus google-chrome visual-studio-code spotify
```

They are the most important software I use :

- slack is my office
- zoom is my phone
- google chrome and  vscode are my axes 
- and spotify
They are the most important software for me , evrything else can be installed from google chrome

## Programing Softwares

### Python
For me as,  I'm a pyhton developer I will install everything related to python :

- let install python3 and python 2


- python 2 I  use ```brew install python@2```

- python 3 I'm a big fan of python 3.6.5 because for now it's the only version supported by tensorflow.

``` brew install --ignore-dependencies https://raw.githubusercontent.com/Homebrew/homebrew-core/f2a764ef944b1080be64bd88dca9a1d80130c558/Formula/python.rb ```
#### JavaScript

Node the lastest version ```brew install node```

#### iterm

The default developer terminal is not for developer , I prefer to use iterm , here is how we install it :

```brew cask install iterm2```


### Customize Terminal 

Let me our terminal look like a developer terminal 

One the big mistake I made from my last laptop was not to save my .zshrc configuration file , and I pay the high price today because I have to customize it twice and write all my alias.

For a fast configuration I decided to use this [blog post](https://dev.to/aspittel/my-terminal-setup-iterm2--zsh--30lm) , it's very helpfull 

