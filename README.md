# Selfhosted-Apps-Docker

###### guide-by-example

---

![logo](https://i.imgur.com/u5LH0jI.png)

---

* [caddy_v2](caddy_v2/) - reverse proxy
* [bitwarden_rs](bitwarden_rs/) - password manager
* [bookstack](bookstack/) - notes and documentation
* [borg_backup](borg_backup/) - backup utility
* [ddclient](ddclient/) - automatic DNS update
* [dnsmasq](dnsmasq/) - DNS and DHCP server
* [homer](homer/) - homepage
* [nextcloud](nextcloud/) - file share & sync
* [portainer](portainer/) - docker management
* [prometheus_grafana](prometheus_grafana/) - monitoring
* [watchtower](watchtower/) - automatic docker images update
* [arch_linux_host_install](arch_linux_host_install)

The core of the setup is Caddy reverse proxy.</br>
It's described in most details.

# Some docker basics and some info

### Compose and environment variables

When making changes use `docker-compose down` and `docker-compose up -d`,
not just restart or stop/start.

* You **do not** need to fuck with `docker-compose.yml` to get something up,
simple copy paste should suffice.

* You **do** need to fuck with `.env` file, that's where all the variables are.
  
Often the `.env` file is used as `env_file`

`env_file: .env`

* `.env` - actual name of a file that is used only by compose.</br>
  It is used automatically just by being in the directory
  with the `docker-compose.yml`</br>
  Variables in it are available during the building of the container,
  but unless named in the `environment:` option, they are not available
  in the running containers.
* `env_file` - an option in compose that defines an existing external file.</br>
  Variables in this file will be available in the running container,
  but not during building of the container.

Benefit is that you do not need to make changes at multiple places,
adding variable or changing its name in `.env` does not require
to also go in to compose to add/change it there..</br>
Also the compose file looks less cramped.

Only issue is that **all** variables from `.env` are available in
containers that use this `env_file: .env` method.</br>
That can lead to potential issues if you try to use this approach elsewhere,
universally.

---

### Images latest tag

All images are without any tag, which defaults to `latest` tag being used.</br>
This is [frowned upon](https://vsupalov.com/docker-latest-tag/),
but feel free to choose a version and sticking with it once it goes to real use.

---

### Bind mount

No docker volumes are used. Directories and files from the host
are bind mounted in to containers.</br>
Don't feel like I know all of the aspects of this,
but I know it's easier to edit a random file on a host,
or backup a directory when it's just there, sitting on the host.

---

### SendGrid

For sending emails free sendgrid account is used, which provides 100 free emails
a day.

The configuration in `.env` files is almost universal, `apikey` is
really the username, not some placeholder.
Only the password(actual value of apikey) changes,
which you generate in apikey section on SendGrid website.

---

### Cloudflare

For managing DNS records. The free tier provides lot of managment options and 
benefits. Like proxy between your domain/subdomain and your server, so no one
can get your public IP just from your domain name. Or 5 firewall rules that allow
you to geoblock whole world except your country.

[How to move to cloudflare.](https://support.cloudflare.com/hc/en-us/articles/205195708-Changing-your-domain-nameservers-to-Cloudflare)
