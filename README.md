# macbook-setup

Upon getting a new Macbook, there's a laundry list of onboarding / setup / configuration steps to make it work for you.

This document aims to ease the discovery process and get you going as painlessly as possible.

# Who this page is for

## New Unix users

If you have little to no experience with any Unix-like system (MacOS / Linux / HP OS / embedded systems / esoteric weirdness), only follow through the next section, OS Setup.
*DO NOT PROCEED past OS Setup, Thar Be Dragons!*
After getting some hands-on XP with the command line, feel free to proceed, but tread lightly!

## New Macbook users

Working on a Macbook for the first time? *Congrats* and welcome to the journey :slight_smile:

Following the initial learning curve and around 1-2 years of use, you'll begrudgingly concede MacOS isn't quite as offensive to your sensibilities anymore.
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

# Terminal Environment Basic Setup

```
# Open terminal and verify your user directory
whoami # output should be <YOUR USER NAME>
pwd  # output should be "/Users/<YOUR USER NAME>"

# Install Xcode manager - xcode-select
xcode-select --install

#Install MacOS package manager - Homebrew
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

```
# Install shell integration, including utilities and Xcode updates (takes awhile, depends mostly on internet speed)
curl -L https://iterm2.com/shell_integration/install_shell_integration_and_utilities.sh | bash

# Set iTerm2 to use zsh from Homebrew instead of Mac default zsh:
brew install -f zsh
echo /opt/homebrew/bin/zsh | sudo tee -a /etc/shells  # provide password
chsh -s /opt/homebrew/bin/zsh  # provide password
```
