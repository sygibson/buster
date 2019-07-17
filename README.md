# Buster
Debian 10 (Buster) Digital Rebar Provision Content Pack

This is a mini-content pack for use with Digital Rebar Provision.  It
adds Debian 10 (Buster) support.

At the moment - this does not work.  It appears that the current ``net.seed``
preseed template used with Digital Rebar Provision causes the Buster installer
to interactively request the Mirror hostname and directory information.  This
did not occur with previous versions of Debian.

I added in a customer preseed:

  * net-seed-buster.tmpl

Which contains the following (template rendered):

  d-i mirror/protocol string https
  d-i mirror/http/hostname string mirrors.edge.kernel.org
  d-i mirror/http/directory string /debian

(Controlled with Params to set these values if desired).

This causes the preseed install to completely fail.

If the stock preseed (`net-seed.tmpl`) is used, then the operator will
have to answer the mirror questions interactively.  The rest of the
install works after that point.

Until this is resolved - Buster is Busted.


NOTE:  If you attempt to work with the `net-seed-buster.tmpl` you can
make changes to it (or clone it), then reference it with the `select-kickseed`
param:
https://provision.readthedocs.io/en/tip/doc/faq-troubleshooting.html?highlight=select%20kickseed#custom-kickstart-and-preseeds

How to use this content pack:

cd content
drpcli contents bundle ../debian-10.yaml
drpcli -E https://<ENDPONT_NAME_OR_ADDRESS>:8092 -U <USERNAME> -P <PASSWORD> contents upload ../debian-10.yaml

NOTE:  the "debian-10.yaml" file in the base directory is the result of the "bundle"
operation listed above.  If you make changes to the contend, you need to "re-bundle"
before uploading.


Set your machine to discovery workflow (Sledgehammer), then run the `debian-10` workflow on it.


