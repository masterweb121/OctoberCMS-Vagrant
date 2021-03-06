# October CMS-Vagrant

A basic Ubuntu 12.04 Vagrant setup with October CMS.

For more information on: [October CMS](http://octobercms.com/)

## Requirements

* VirtualBox - Free virtualization software [Download Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* Vagrant **1.3+** - Tool for working with virtualbox images [Download Vagrant](https://www.vagrantup.com)
* Git - Source Control Management [Download Git](http://git-scm.com/downloads)

## Setup

* Clone this repository `git clone https://github.com/runtheyards/OctoberCMS-Vagrant.git`
* run `vagrant up` inside the newly created directory where the Vagrant file is present
* (the first time you run vagrant it will need to fetch the virtual box image which is ~300mb so depending on your download speed this could take some time)
* Vagrant will then use puppet to provision the base virtual box with our LAMP stack (this could take a few minutes) and will fetch the installation files for October CMS
* You can verify that everything was successful by opening http://localhost:8888/install.php in a browser and follow the on screen instructions to setup October CMS

## Usage

Some basic information on interacting with the vagrant box

### Port Forwards

* 8888 - Apache
* 8889 - MySQL 
* 5433 - PostgreSQL

If you have other stuff such as MAMP running on port 8888 vagrant will automatically assign different ports. To find out which ones have been used have a look in the console readouts after you have `vagrant up`. It should be somewhere near the top.

### Default MySQL/PostgreSQL Database

* User: root
* Password: (blank)
* DB Name: database


### PHPmyAdmin

Accessible at http://localhost:8888/phpmyadmin using MySQL access credentials above.

### Sequel Pro Settings
if you wan to use Sequel Pro instead of PHPmyAdmin use these settings.

* MySQL Host: 127.0.0.1
* Username: root
* Password: (blank)
* SSH Host: 127.0.0.1
* SSH User: vagrant
* SSH Key: ~/.vagrant.d/insecure_private_key (Click the key and it'll ask you to locate the key)
* SSH Port: 2222

### MailCatcher & October CMS
[MailCatcher] (http://mailcatcher.me/) has been configured as is quite handy when testing email functionality and contact forms within a local environment. 

To access MailCatcher enter http://0.0.0.0:1080/ in the browser (this has been enabled during the setup).

Details:

* smtp://0.0.0.0:1025
* http://0.0.0.0:1080

To configure MailCatcher within October CMS you will need to update the app/config/mail.php file with the smtp details. Note encryption has been set to null, you would probably want to restore this to the original value on production servers. 

`'driver' => 'smtp',`

`'host' => '0.0.0.0:1025',`

`'port' => 1025,`

`'encryption' => '',`

Mail catcher runs as a deamon so will run in the background, it can be disabled within the dashboard at http://0.0.0.0:1080/ or you can `vagrant ssh` into the box and run the following command:

`ps ax | grep mailcatcher` 

This will output something along these lines:

22183 ?        S      0:01 /opt/vagrant_ruby/bin/ruby /opt/vagrant_ruby/bin/mailcatcher --ip=0.0.0.0
22216 pts/0    S+     0:00 grep --color=auto mailcatcher

In the above example to kill MailCactehr enter:

`kill 22183`

### Vagrant

Vagrant is [very well documented](https://docs.vagrantup.com/v2/) but here are a few common commands:

* `vagrant up` starts the virtual machine and provisions it
* `vagrant suspend` will essentially put the machine to 'sleep' with `vagrant resume` waking it back up
* `vagrant halt` attempts a graceful shutdown of the machine and will need to be brought back with `vagrant up`
* `vagrant ssh` gives you shell access to the virtual machine


----
##### Virtual Machine Specifications #####

* OS     - Ubuntu 12.04
* Apache - 2.4.6
* PHP    - 5.5.4
* MySQL  - 5.5.32
* PostgreSQL - 9.1
