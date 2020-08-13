# Quickly setup a Virtual Machine instance using Ubuntu

- [Quickly setup a Virtual Machine instance using Ubuntu](#quickly-setup-a-virtual-machine-instance-using-ubuntu)
  - [Update package manager](#update-package-manager)
  - [Setup emacs](#setup-emacs)
  - [Setup some aliases for the command line](#setup-some-aliases-for-the-command-line)
  - [Add useful git aliases](#add-useful-git-aliases)
  - [Prettify bash command prompt](#prettify-bash-command-prompt)
  - [Install Conda and Python 3](#install-conda-and-python-3)
  - [Setup Chrome Remote Desktop](#setup-chrome-remote-desktop)
  - [Set up your SSH keys to connect to your instance](#set-up-your-ssh-keys-to-connect-to-your-instance)

## Update package manager

```bash
sudo apt-get update
sudo apt-get upgrade -y
```

## Setup emacs

```bash
cd ~
sudo apt install emacs
emacs .emacs
```

Then copy and paste the contents of [this](.emacs) file into `.emacs` for a better development experience.

## Setup some aliases for the command line

Add this to your `.bashrc`:

```bash
alias ll='ls -alhF'
alias l='ls -F'
```

`-F` is for adding things like a slash ("/") in front or directories, etc. `-h` to print sizes in a readable format, `a` to also print files starting with `.`, and `-l` to list in long format.

## Add useful git aliases

```bash
git config --global alias.logline "log --all --decorate --oneline --graph"
git config --global alias.cdiff "diff --color-words"
```

## Prettify bash command prompt

// TODO

## Install Conda and Python 3

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
bash Anaconda3-2019.10-Linux-x86_64.sh
conda create -n dev anaconda
conda install -c anaconda cmake
```

You can add this to your `.bashrc` to always be on the `dev` virtual environment on startup by default.

```bash
conda activate dev
```

Then close and open the terminal or run:

```bash
source .bashrc
```

## Setup Chrome Remote Desktop

[Install and setup Chrome Remote Desktop](https://cloud.google.com/solutions/chrome-desktop-remote-on-compute-engine) along with Google Chrome in your remote instance to control your VM through a GUI desktop environment.

## Set up your SSH keys to connect to your instance

If you don't have your own SSH keys setup in your local machine, do:

```bash
ssh-keygen -t rsa -f ~/.ssh/[KEY_FILENAME] -C [USERNAME]
```

- [KEY_FILENAME] is the name that you want to use for your SSH key files. For example, a filename of `my-ssh-key` generates a private key file named `my-ssh-key` and a public key file named `my-ssh-key.pub`.
- [USERNAME] is the username for the user connecting to the instance.

This command generates a private SSH key file and a matching public SSH key (at ~/.ssh/[KEY_FILENAME].pub) with the following structure:

```bash
ssh-rsa [KEY_VALUE] [USERNAME]
```

Restrict access to your private key so that only you can read it and nobody can write to it.

```bash
chmod 400 ~/.ssh/[KEY_FILENAME]
```

Then go to the dashboard and edit your VM instance. Copy the contents of your public key file wherever you have to add your keys in your instance's configuration dashboard.

Then, to connect to your instance from your local machine, do:

```bash
ssh -i ~/.ssh/[KEY_FILENAME] [USERNAME]@[EXTERNAL_IP_ADDRESS_OF_YOUR_INSTANCE]
```

Before doing this last step, you might want to assign your VM instance a permanent external IP address, so that the host can be remembered by your local machine.

Note: I highly recommend you use VS code to connect to your remote VM instance. If you don't have it aready, install the `Remote - SSH` VS code extension. Then by typing the last `ssh` command above open your remote through VS code.
