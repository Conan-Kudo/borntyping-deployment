deployment
==========

Deploys dotfiles, configuration and packages to my machines using Ansible.

Usage
-----

Download the repository, create an inventory file, and run the `./ansible-run` script.

### Installing ansible

Install the latest ansible release with:

```bash
sudo apt-add-repository --yes ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible git
```

### Fetch this repository

Clone this repository:

```bash
mkdir -p ~/Development/borntyping
cd ~/Development/borntyping
git clone git@github.com:borntyping/deployment.git
cd deployment
```

### Create an inventory file

Create an inventory file named `./inventory.conf`. An example inventory might look like this:

```
[workstation]
localhost ansible_connection=local ansible_sudo_pass=EXAMPLE

[servers]
remotehost ansible_ssh_host=remotehost.example.co.uk ansible_ssh_port=22 ansible_sudo_pass=EXAMPLE
```

If you don't want to create an inventory file, and only want to configure the local machine, you can pass `--inventory=localhost,` as an argument to `./ansible-run`.

### Run ansible

```bash
./ansible-run
```

Bootstrapping a new server
--------------------------

If the OS was not created with a personal user, create one before running the above instructions:

```bash
adduser sam
usermod -a -G sudo sam
```

You can the either run Ansible from your workstation by adding a remote connection to the inventory, or directly on the machine you are bootstrapping. To run Ansible directly on the machine:

```bash
ansible-pull --url=git@github.com:borntyping/deployment.git -i localhost, -K site.yml
```

Once ansible has run, set passwords for any additional users:

```bash
sudo passwd USER
```

Development
-----------

Some dependencies are managed using [Peru](https://github.com/buildinspace/peru), which is installed by the `stage2-user` role. Once the development machine is bootstrapped you can run `peru reup` to update those files, which will fetch the lastest versions of the dependecies and copy them into this repository. Tasks that install these files are tagged with `peru`, so updating the deployed versions of those files can be done with `./ansible-run -t peru`.

Licence
-------

The MIT License (MIT)

Copyright (c) 2014 Sam Clements

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
