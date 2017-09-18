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
