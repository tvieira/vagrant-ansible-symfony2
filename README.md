Vagrant + Ansible + Symfony 2
=============================

This project is based on [Matt Wright's vagrant-ansible-tutorial](https://github.com/mattupstate/vagrant-ansible-tutorial) and I built it to help me when I have to build PHP apps with Symfony 2 framework. The idea is more on coming up with a quick dev environment instead of the application itself, that's why I'm using a standard installation of Symfony 2.

I haven't changed the structure Matt has built (even the names of the scripts were kept the same). I've added/amended/removed few ansible actions to match the PHP and Symfony2 need. The idea is to have two servers (web and database) for a quick symfony 2 development environment so you can crack on your new app using Symfony 2.

Requirements
------------
This template requires that you have installed:

- [Vagrant](http://vagrantup.com) (I'm using version [1.2.2](http://downloads.vagrantup.com/tags/v1.2.2))
- [Ansible](http://www.ansibleworks.com/docs/gettingstarted.html) (I'm using version 1.2.2)
- [Openssl](http://www.openssl.org/) (There is one certificate already in the template. Use this tool only if you want to use your own certificate - see below **_Creating certificate for HTTPS_**. I'm using the latest version installed via Macports)

Environment
-----------

I have been using this template on a Mac OS X 10.7.5 and I'm assuming you know how to install the apps above. The Symfony version that I'm using for this template is the [Standard 2.3.2 without vendors](http://symfony.com/download). The configuration for the php, mysql and nginx are quite similar (if not similar) to the default configuration that comes with each package when you install via Ubuntu.

Install &amp; Run
-----------------

1. First clone this project

        git clone git@github.com:tvieira/vagrant-ansible-symfony2.git

2. Create both, web and db, vagrant servers

        vagrant up web && vagrant up db

    <small>\*_During the _'GATHERING FACTS'_ process you may have to say _'yes'_ to confirm authenticity of the host you have created (for web and db)_</small>

3. Deploy the app

        devops/deploy.sh

4. Check

    + Symfony 2 Dev Home Page: [http://localhost:8080/app_dev.php/](http://localhost:8080/app_dev.php/)
    + Symfony 2 Dev SSL login page: [https//localhost:4443/app_dev.php/](https//localhost:4443/app_dev.php/)

Optionals
=========

Creating certificate for HTTPS
------------------------------

Explaining the actions below is beyond the subject of this tutorial. The project already have a certificate to get it up and running asap, but if you want to create a new certificate, the instructions are shown below.

1. Create the key

        openssl genrsa -des3 -out tutorial.key 1024

2. Create the certificate signing request

        openssl req -new -key tutorial.key -out tutorial.csr

3. For the sake of simplicity and development, lets remove the password from the key so we have a certificate without password.

        mv tutorial.key tutorial.pwd.key
        openssl rsa -in tutorial.pwd.key -out tutorial.key

4. Create the certificate

        openssl x509 -req -days 3650 -in tutorial.csr -signkey tutorial.key -out tutorial.crt

Now just move the 3 files you just created (key, csr and crt) into the devops folder so the script can deploy it for you.

Todo
====

- it seems a bit odd to have all the libraries installed (see devops/webserver.yml). I should make that shorter.
- Is it possible to use the action _"remove app\_dev restriction"_ within deploy.yml to look at the correct IP of the developer machine so we add it to the list of permitted IPs and we don't need to remove the app_dev.php restriction?
- last action within deploy.yml _"ensure right dev permission"_ is needed because for every deployment the permissions on cache/dev is changed to root (I'm not saying it is right or wrong, but it must have a better way to execute this)

Is there anything else that could be done to improve this template? I'm sure there are plenty of room to optimize this environment (including tweaking the default configuration files for mysql, php, etc). You can help me if you are interested.