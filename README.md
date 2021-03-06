# A Virtual Machine for Ohana Web Search Development

## Introduction
This project automates the setup of a development environment for working on Ohana Web Search. Use this virtual machine to work on a pull request with everything ready to hack and run the test suites.

## Windows installation
* [Follow the Windows Installation Guide wiki page](https://github.com/codeforamerica/ohana-weh-search-dev-box/wiki/Windows-ohana-web-search-dev-box-installation-guide).

## Mac OSX installation
* Follow the directions below...

### Requirements
* [VirtualBox](https://www.virtualbox.org)

* [Vagrant 1.1+](http://vagrantup.com) (not a Ruby gem)

### How To Build The Virtual Machine
1. **Install Vagrant**, which can be [downloaded](http://www.vagrantup.com/downloads.html) from the Vagrant site, which also provides [step-by-step installation instructions](http://docs.vagrantup.com/v2/getting-started/index.html).

2. **Build the VM**

  In the directory you want to work in, enter the following:

  ```
  $ git clone https://github.com/codeforamerica/ohana-web-search-dev-box
  $ cd ohana-web-search-dev-box
  $ vagrant up
  ```

If the base box is not present, `vagrant up` fetches it first.

After the installation has finished (it can take several minutes), you can access the virtual machine with the following command:

    host $ vagrant ssh
    Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic-pae i686)
    ...
    vagrant@ohana-web-search-dev-box:~$

`host $` refers to the command prompt on your computer's OS, as opposed to the prompt in the virtual machine.

Port 8080 in the host computer is forwarded to port 8080 in the virtual machine. Thus, applications running in the virtual machine can be accessed via localhost:8080 in the host computer.

### What's In The Box
* Git

* RVM

* Ruby 2.1.3 (binary RVM install)

* Bundler

* System dependencies for nokogiri and pg

* Node.js for the asset pipeline

* PhantomJS

### Recommended Workflow
The recommended workflow is

* edit in the host computer (i.e. your physical computer)

and

* test within the virtual machine.

This workflow is convenient because in the host computer you normally have your editor of choice fine-tuned, Git configured, and SSH keys in place.

### Set up the project
Clone your ohana-web-search fork into the ohana-web-search-dev-box directory on the host computer:

    host $ ls
    LICENSE.md  README.md  Vagrantfile  bootstrap.sh
    host $ git clone https://github.com/<your GitHub username>/ohana-web-search.git

#### Set up the environment variables

Inside the `config` folder, you will find a file named `application.example.yml`. Copy its contents to a new file in the same directory called `application.yml`.

Verify that you can launch the app:

    vagrant@ohana-web-search-dev-box:/vagrant/ohana-web-search$ rails s -p 4000

You should now be able to access the app on the host machine at
http://localhost:4000

#### Test the app

Run tests in the virtual machine with this simple command:

    vagrant@ohana-web-search-dev-box:/vagrant/ohana-web-search$ script/test

## Virtual Machine Management

When done just log out with `^D` (or `logout`) and suspend the virtual machine

    host $ vagrant suspend

then, resume to hack again

    host $ vagrant resume

Run

    host $ vagrant halt

to shutdown the virtual machine, and

    host $ vagrant up

to boot it again.

You can find out the state of a virtual machine anytime by invoking

    host $ vagrant status

Finally, to completely wipe the virtual machine from the disk **destroying all its contents**:

    host $ vagrant destroy # DANGER: all is gone

Please check the [Vagrant documentation](http://docs.vagrantup.com/v2/) for more information on Vagrant.

## Faster test suites

The default mechanism for sharing folders is convenient and works out the box in
all Vagrant versions, but there are a couple of alternatives that are more
performant.

### rsync

Vagrant 1.5 implements a [sharing mechanism based on rsync](https://www.vagrantup.com/blog/feature-preview-vagrant-1-5-rsync.html)
that dramatically improves read/write because files are actually stored in the
guest. Just throw

    config.vm.synced_folder '.', '/vagrant', type: 'rsync'

to the _Vagrantfile_ and either rsync manually with

    vagrant rsync

or run

    vagrant rsync-auto

for automatic syncs. See the post linked above for details.

### NFS

If you're using Mac OS X or Linux you can increase the speed of the test suite with Vagrant's NFS synced folders.

With an NFS server installed (already installed on Mac OS X), add the following to the Vagrantfile:

    config.vm.synced_folder '.', '/vagrant', type: 'nfs'
    config.vm.network 'private_network', ip: '192.168.50.4' # ensure this is available

Then

    host $ vagrant up

Please check the Vagrant documentation on [NFS synced folders](http://docs.vagrantup.com/v2/synced-folders/nfs.html) for more information.

## License

Copyright (c) 2014–<i>ω</i> Code for America. See [LICENSE](https://github.com/codeforamerica/ohana-web-search-dev-box/blob/master/LICENSE.md) for details.
