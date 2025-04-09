---
title: macOS Setup
authors: [silentglasses]
date: 2021-12-22 06:46:00
categories: [Hardware, macOS]
tags: [mac, laptop, build]
---

Setting up a new Mac for tech work involves a series of steps to ensure that your system is secure, efficient, and equipped with the necessary tools for your development needs. This guide will walk you through essential system changes, hardening your Mac for security and privacy, and suggest applications for general tech work, Python, Ruby on Rails, and Go development. Additionally, we'll recommend some utilities that are highly beneficial for tech professionals.
<!-- more -->

## System Changes

=== "Update macOS"
    Ensure your macOS is up-to-date to benefit from the latest features and security patches. Go to `System Preferences` > `Software Update`, and install any available updates.
=== "System Preferences"
    Customize your system settings to improve efficiency:
    - **Dock:** Reduce the size, enable auto-hide, and remove unnecessary apps.
    - **Trackpad:** Enable tap to click and configure gestures for better productivity.
    - **Keyboard:** Increase key repeat rate and reduce delay until repeat.
    - **Hot Corners:** Set up hot corners to access Mission Control, Desktop, or other frequently used features quickly.
=== "FileVault"
    Enable FileVault to encrypt your disk and protect your data. Go to `System Preferences` > `Security & Privacy` > `FileVault`, and turn it on.
=== "Firewall"
    Turn on the firewall to protect your Mac from unauthorized access. Go to `System Preferences` > `Security & Privacy` > `Firewall`, and enable it.
=== "Dock"
    - Adjust size and Position as you find convenient. I make mine smaller and leave it at the bottom.
    - check **Minimize windows into application** icon
=== "Trackpad"
    - Click the **Point & click** tab.
    - Change Secondary click to Right corner
    - Enable Tap to click with one finger
    - Adjust Tracking speed as needed
=== "Finder"
    - Finder → Preferences
        - Change **New finder window show** to open in your Home Directory
        - Sidebar:
            - Check **Home**
            - Uncheck **Recent Tags**
    - View:
        - Click **Show Path Bar**
    		- Click **Show Status Bar**
    		- Click **Show View Options**:
    		    - Set **Always open in column view**
    		    - Set **Browse in column view**
    				- Set Group by to **Kind**
    				- Set Sort by to **Name**

## Hardening for Security and Privacy

=== "Privacy Settings"
    Review and configure privacy settings to control app permissions. Go to `System Preferences` > `Security & Privacy` > `Privacy`.
=== "Password Management"
    Use a password manager like 1Password or Bitwarden to store and manage your passwords securely.
=== "VPN"
    Install a reputable VPN service like NordVPN or ExpressVPN to encrypt your internet traffic and protect your privacy.

## Suggested Applications for General Tech Work

=== "Homebrew"
    Install Homebrew, the macOS package manager, to simplify the installation of software:

    ```sh
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
=== "iTerm2"
    iTerm2 is a powerful terminal emulator that offers features like split panes, hotkeys, and extensive customization.

    ```
    brew install iterm2
    ```
=== "Visual Studio Code"
    Visual Studio Code is a versatile and powerful code editor with a vast library of extensions for various programming languages.

    ```
    brew install visual-studio-code
    ```
=== "Coding Fonts"
    Pick one of the following fonts to use, these include ligatures for powerlevel10k. I use Hack.

    ```
    brew install font-fira-code-nerd-font
    brew install font-fira-mono-nerd-font
    brew install font-hack-nerd-font
    brew install font-roboto-mono-nerd-font
    brew install font-ubuntu-mono-nerd-font
    ```

## Recommended Utilities

=== "Rectangle"
    Rectangle is a window management tool that allows you to organize your windows using keyboard shortcuts.

    ```
    brew install rectangle
    ```
=== "appcleaner"
    [AppCleaner](https://freemacsoft.net/appcleaner/) - uninstall unwanted apps
=== "Slack"
    Slack is a collaboration tool that helps you communicate with your team, share files, and integrate with various services.

    ```
    brew install slack
    ```
=== "Little Snitch"
    Little Snitch is a network monitor that helps you control and monitor your network traffic.
=== "angry-ip-scanner"
    [Angry IP Scanner](https://angryip.org/) - Open-source, cross-platform network scanner
=== "Bartender"
    Bartender allows you to organize your menu bar icons, hiding those you don’t need and keeping your menu bar tidy.

## Your shell

=== "Oh My Zsh"
    Oh My Zsh is an open source, community-driven framework for managing your zsh configuration.

    ```
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    ```
    - Change your shell, the zsh install should ask you to and take care of this, but just in case:
    ```
    chsh -s $(which zsh)
    ```
    - Start a new shell instance"
    ```
    source ~/.zshrc
    ```
=== "Powerlevel10k"
    I use this utility to add extra features to my shell, see [here](https://github.com/romkatv/powerlevel10k) for instructions.

    ```
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    ```

    - Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`
=== "zsh Plugins"
    Additional plugins can be found at [zsh-users ](https://github.com/zsh-users) and [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins)

    ```
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    ```

    ```
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    ```

    ```
    git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
    ```

    Add the plugins to your `.zshrc` file like so:

    ```
    # zsh Plugins
    plugins=(
      [...]
      zsh-autosuggestions
      zsh-completions
      zsh-syntax-highlighting
    )
    ```
=== "tree"
    ```
    brew install tree
    ```
=== "vim"
    ```
    brew install vim
    ```
=== "htop"
    ```
    brew install htop
    ```

## Development Environment Setup

See our other blog posts on setting up the Python, Ruby/Rails and Go.
