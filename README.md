# Packer Debian9 Docker

Scripts to build a virtual box with Docker on Debian9 and publish to vagrant cloud.

Can be used as a template to provision your own boxes so you don't have to install
lots of software when you do an initial vagrant up.

Run:

`packer build debian9.json`

If it fails chances are high that the Debian iso has been updated.

Look at the following lines in debian9.json

```
      "iso_urls": [
        "iso/debian-9.1.0-amd64-xfce-CD-1.iso",
        "https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.1.0-amd64-xfce-CD-1.iso"
      ],
      "iso_checksum_type": "sha256",
      "iso_checksum": "9558262f6c15b024d8c3ca140d1c70c310f11384e227329d08dab38c915a4306",
```

Have a look at https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/ and update the url and checksums.

## Create your own box

* Signup for [Vagrant Cloud](https://app.vagrantup.com) and sign in
* Click into the 'Security' tab
* Create an Authentication Token
* Create a local env in your profile, e.g. `export VAGRANTCLOUD_TOKEN=TOKEN_FROM_ABOVE`
* Click 'Dashbord' then 'New Vagrant Box'
* Clone this repo
* Edit debian9.json and replace `"box_tag": "openbuild/debian-9-docker"` with your box from the step above
* Build the box with `packer build debian9.json`

### Amending the software

In a new directory on your machine:

Create file `Vagrantfile`

```
Vagrant.configure(2) do |config|
  config.vm.box = "openbuild/debian-9-docker"
  config.vm.provider "virtualbox" do |v|
  end
  config.vm.network :public_network, :public_network => "wlan0"
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
  end
end
```

Create file `ansible/playbook.yml`

```
# Look at the roles in this repo for examples (copy/paste/amend)
---
- hosts: all
  become_user: root
  roles:
    - users
    - vim
    - docker
```

Run `vagrant up`

Fix any issues and add more roles etc, then `vagrant provison` until it fully works and you are happy with the box state.

Once you have a local box up and running copy the roles into this repo and add them to `role_paths` in `debian9.json` you should then be able to run `packer build debian9.json` to publish your box.
