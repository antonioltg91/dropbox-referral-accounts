# Dropbox Referral Accounts

I believe it should be easy and painless to use Dropbox, and that not having enough available space is a pain.

I created this package to relieve you from this pain, by giving you up to 16 Gigabytes of space for any Dropbox account
of your choice, without requiring efforts or manual intervention.

How ? By automating the creation and linking of random *referral accounts* to a Dropbox account of your choice.

For each referral account, your space will be increased of 500 Mb. Do it 32 times, and you're up the maximum of 16 Gb
allowed by Dropbox.

## Prerequisites

Install up-to-date versions of [Vagrant](https://www.vagrantup.com/downloads.html) and
[VirtualBox](https://www.virtualbox.org/wiki/Downloads).

## Installation

1. Create a new folder on your computer : `mkdir -p ~/dropbox-referral-accounts && cd $_`.
2. Download the scripts inside it :
  `wget -O - https://bitbucket.org/gnutix/dropbox-referral-accounts/get/master.tar.gz | tar xzf - --strip 1`
3. Create your own configuration file : `cp config/config.cfg.dist config/config.cfg`
4. Adapt the following values of `config/config.cfg` to your needs :

```
# The referral link of the Dropbox account for which you want to increase the storage.
dropboxReferralLink = "https://db.tt/xxxxxxxx"
 
# The referral accounts' email. "%d" will be replaced by the account ID from the loop.
accountEmail = "my.gmail.nickname+change_me_for_something_unique.%d@gmail.com" 
```

## Give me space !

Execute `bash run.sh 1 32` in your terminal and watch the magic happen
(PS: you will be prompted for your sudo password by Vagrant, so don't run the command and leave, or the magic will die).

Please note that running the script the first time will take you a while, as it has to download a Vagrant box of ~500 Mb.

## Troubleshooting

### DHCP conflict

If you see the following message :

> A host only network interface you're attempting to configure via DHCP already has a conflicting host only adapter with
> DHCP enabled. The DHCP on this adapter is incompatible with the DHCP settings. Two host only network interfaces are not
> allowed to overlap, and each host only network interface can have only one DHCP server. Please reconfigure your host
> only network or remove the virtual machine using the other host only network.

Change the following line of the file `Vagrantfile` :

```
    config.vm.network :private_network, type: "dhcp"
```
to
```
    config.vm.network :private_network, ip: "10.10.10.10"
```
(you are free to use any IP address in the
[private IP address space](http://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces) instead of
`10.10.10.10`).

### Accounts not correctly created or linked

You can take a look at the `screenshots/` folder, which contains screenshots done just before errors occurs.
You can also display more information when running the script, by using `logging = "debug"` in the configuration file.

One issue that happens a lot is that the JavaScript on Dropbox website isn't loaded fast enough, as it highly depends on
your network connection, especially through TOR's proxy. Increasing CasperJS's timeout by changing the `timeout`
configuration key can help.

Anyway, keep in mind that browser scraping is "fragile", and unfortunately doesn't work 100% of the time.

## Technical information

### The nasty details

The scripts :

* creates temporary Vagrant boxes with varying MAC addresses
* installs and run a TOR proxy for varying IP addresses
* installs and run Dropbox's software on them
* crawls Dropbox's website using CasperJS :
    1. to create a random referral account, from the configured referral link
    2. to link the referral account with the Dropbox software installed in the box

### Advanced usage

The script can be used in two ways :

* handling multiple accounts (`bash run.sh X Y`)
* handling one account (`bash run.sh X`)

Take a closer look at the configuration file for more options.

## Disclaimer

The author of this script disclaims all responsibility for the effects of its usage by whomever other than himself.
