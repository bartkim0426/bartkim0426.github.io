---
layout: post
title:  "How to use Valut with django"
categories: tools etc
tags: vault
---

# How to use Vault with django

# Starting Vault



## Installing Vault
You can follow official site [tutorial](https://www.vaultproject.io/intro/getting-started/install.html). It's really easy to install.   
1. download vault [here](https://www.vaultproject.io/downloads.html)
2. unzip the package
3. check vault binary is avalible in PATH     

```bash
# inside dir vault file is
# it's in ~/Downloads now
mkdir ~/vault
mv ~/Downloads/vault ~/vault

# add PATH to ~/.profile file
export PATH=$PATH:/Users/username/vault
```
Now you can use `vault` in terminal




## Starting with dev server
```bash
$ vault serve -dev
WARNING: Dev mode is enabled!

In this mode, Vault is completely in-memory and unsealed.
Vault is configured to only have a single unseal key. The root
token has already been authenticated with the CLI, so you can
immediately begin using the Vault CLI.

The only step you need to take is to set the following
environment variable since Vault will be talking without TLS:

**# YOU SHOULD COPY THIS**
    export VAULT_ADDR='http://127.0.0.1:8200'

	The unseal key and root token are reproduced below in case you
	want to seal/unseal the Vault or play with authentication.

**# AND THOSE KEY (Unseal Key, Root Token)**
	Unseal Key: 2252546b1a8551e8411502501719c4b3
	Root Token: 79bd8011-af5a-f147-557e-c58be4fedf6c

	==> Vault server configuration:

		 Log Level: info
			Backend: inmem
		Listener 1: tcp (addr: "127.0.0.1:8200", tls: "disabled")
...
```

1. Launch a new termianl
2. copy `export VAULT_ADDR='http://127.0.0.1:8200'` above and paste to command
3. save `Unseal Key` and `Root Token`
4. you can check server by `vault status`



## Basic usage

**writing secret**

```bash
$ vault write secret/first value=secret
Success! Data written to: secret/first
# you can write multiple piece of data too
$ vault write secret/first vaule=secret myname=seul
```

**read secret**

```bash
# easily read secrets
$ vault read secret/first

# you can use json format
$ vault read -format=json secret/first

# also you can use jq tool to extract only value
$ vault read -format=json secret/hello | jq -r .data.myname
```
> If you want to use jq tool, you have to install it. check [here](https://stedolan.github.io/jq/download/) to install   
> easily install in mac os by `brew install jq`

**Delete secret**
```bash
$ vault delete secret/first
Success! Deleted 'secret/first' if it existed.
```

