# macbook-setup

Upon getting a new Macbook, there's a laundry list of onboarding / setup / configuration steps to make it work for you.

This document aims to ease the discovery process and get you going as painlessly as possible.

# Who this page is for

## New Unix users

If you have little to no experience with any Unix-like system (macOS / Linux / HP OS / embedded systems / esoteric weirdness), only follow through the next section, OS Setup.
*DO NOT PROCEED past OS Setup, Thar Be Dragons!*
After getting some hands-on XP with the command line, feel free to proceed, but tread lightly!

## New Macbook users

Working on a Macbook for the first time? *Congrats* and welcome to the journey :slight_smile:

Following the initial learning curve and around 1-2 years of use, you'll begrudgingly concede macOS isn't quite as offensive to your sensibilities anymore.
MacOS's saving grace is it's lineage of UNIX, making its shells close to Linux shells:

* (Almost) anything can be accessed via the terminal
* A vast package management system extends functionality easily
* Tools like SSH and CURL work out-of-the-box

## Veteran Unix users
We happily accept comments, corrections and contributions.
The Conclusion section is eagerly awaiting your input!

# Onwards!

Now then, *let's get this show on the road!*

# OS Setup
1. Set a reasonably typeable password for your user
2. Upgrade OS version to latest
3. Navigate to keyboard preferences, and set:
    * Key repeat rate (suggestion - set all the way to Fast)
    * Delay until repeat (suggestion - set to one before Short)

![keyboard_preferences.png](assets/keyboard_preferences.png)

# Terminal Environment Setup

## Installation

```shell
# Open terminal and verify your user directory
whoami # output should be <YOUR USER NAME>
pwd  # output should be "/Users/<YOUR USER NAME>"

# Install Xcode manager - xcode-select
xcode-select --install

# Install MacOS package manager - Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
 (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/$(whoami)/.zprofile
 eval "$(/opt/homebrew/bin/brew shellenv)" 

# Install enhanced terminal - iTerm2
brew update 
brew install iterm2 --cask
open -a iTerm .  
```
The last step triggers an OS Popup to allow to open the new iTerm2 terminal. After the popup, you can close the old terminal and continue in iTerm2

Going forward, iTerm2 will be referred to as terminal

```shell
# Install shell integration, including utilities and Xcode updates (takes awhile, depends mostly on internet speed)
curl -L https://iterm2.com/shell_integration/install_shell_integration_and_utilities.sh | bash

# Set iTerm2 to use zsh from Homebrew instead of Mac default zsh:
brew install -f zsh
echo /opt/homebrew/bin/zsh | sudo tee -a /etc/shells  # provide password
chsh -s /opt/homebrew/bin/zsh  # provide password
```
## Config
Below is my personal terminal configs file.

It has some sane defaults, for example Option+arrow is defined to jump words (similar to the default behavior of macOS).

[com.googlecode.iterm2.plist](assets/com.googlecode.iterm2.plist)

To load it press CMD+, for the settings window and then General → Settings → Check the “Load settings from Custom” checkbox

![iterm2_settings_general_settings.png](assets/iterm2_settings_general_settings.png)

Finally make iTerm2 your default terminal in the OS

![iterm2_macos_ribbon.png](assets/iterm2_macos_ribbon.png)

# Useful Tools
## Managers
[hatch](https://hatch.pypa.io/latest/) - The modern, extensible Python project manager with support for `uv` and `ruff`

```shell
brew install hatch uv ruff
```

[direnv](https://direnv.net/) - an environment variable manager
Extremely useful for small local workflow automations

```shell
brew install direnv
```

Note - brew only installs it. Activation happens via Oh-My-Zsh

[Oh-My-Zsh](https://ohmyz.sh/) (OMZ) - zsh framework with a rich plugins and themes ecosystem

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

powerlevel10k - theme for OMZ


```shell
# there's a brew install option for this, but it doesn't work too well. Better just clone from source
ZSH_CUSTOM=${ZSH_CUSTOM:-~/.oh-my-zsh/custom}
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM}/themes/powerlevel10k
sed -i '.bak2' 's/^ZSH_THEME=".*"/ZSH_THEME="powerlevel10k\/powerlevel10k"/g' ~/.zshrc
```

xxx WARNING xxx

After installation fully quit iterm2 and start it again.

Just closing the window isn't enough. Quit it from the OS

xxx xxx xxx xxx

Now configure the theme

```shell
p10k configure
```

OMZ builtin plugins:

* [aws](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/aws) - AWS user profile management
* [brew](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/brew) - MacOS package manager 
* [colored-man-pages](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/colored-man-pages) - add a little zhuzh to man pages 
* [colorize](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/colorize) 
* [fzf](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/fzf) - enable fuzzy auto-completion and key bindings for fzf 
* [git](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git) 
* [iterm2](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/iterm2) 
* [macos](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/macos) 
* [python](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/python) 
* [direnv](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/direnv) - activate direnv 
* [z](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/z) - tracks usage of filepaths and auto-suggests completions by "frecency"

OMZ custom plugins:
* [syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) - highlights installed commands inline
* [autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) - suggests commands as you type based on history and completions

```shell
ZSH_CUSTOM=${ZSH_CUSTOM:-~/.oh-my-zsh/custom}
CLONE_SOURCE=https://github.com/zsh-users
echo "
zsh-syntax-highlighting
zsh-autosuggestions
" | xargs -n 1 -P 5 -I '{}' git clone $CLONE_SOURCE/'{}' $ZSH_CUSTOM/plugins/'{}'
sed -i '.bak1' 's/^plugins=(.*)/plugins=(aws brew colored-man-pages colorize fzf git iterm2 macos python zsh-syntax-highlighting zsh-autosuggestions direnv z)/g' ~/.zshrc 
source ~/.zshrc
grep 'plugins=(.*)' ~/.zshrc | grep -Fv '# Example'
# Last step is verification. Should return:
# plugins=(aws brew colored-man-pages colorize git iterm2 macos python zsh-syntax-highlighting zsh-autosuggestions direnv z)
```

## Utilities
```shell
brew install vim  # override system app with brew package to get the full version
brew install httpie  # for parity with http requests on linux using the http util
brew install jq yq tree  # parsers
brew install azure-cli awscli  # cloud CLI clients
brew install esolitos/ipa/sshpass  # commonly used in Iguazio for server remote access
# Main usage: 
# Double-click on a Provazio remote server IP - his will copy sshpass command to Clipboard. Paste to Terminal and run 
# Recommended general usage:
# Save a password to a file FILENAME and run:
# sshpass -f FILENAME ssh user@remote 
# Example: 
# sshpass -f ~/.password ssh -o StrictHostKeyChecking=no -o ServerAliveInterval=180 -o ServerAliveCountMax=3 iguazio@3.20.72.13 
# Very useful usage examples here - https://www.cyberciti.biz/faq/how-to-install-sshpass-on-macos-os-x/

```
GitHub CLI - install and connect to your account:

1. Connect to your GitHub account via the browser 
2. Install GitHub CLI tool and link your account
```shell
brew install gh
gh auth login
```
