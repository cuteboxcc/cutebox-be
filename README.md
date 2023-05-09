# Cutebox Backend server

This is the cutebox backend server. It is based on GoToSocial and aims to improve on the server software.

It is a direct modification of the release builds so the changes are quite minimal and usually concentrate on UI/UX.

## How to deploy

Deployment of cutebox is very simple. Follow along the tutorial and you will have a proper cutebox instance in no time.

### Raspberry Pi

Raspberry Pi images coming soon which should make deployment even easier.

### Requirements

* Intermediate computer knowledge
* A x86_64 Linux machine to run the server on
* An open port (80 and 443) in your firewall
* Ideally SSH access to the server (will make copying- and pasting commands easier)
* If you're running it at home: a DynDNS provider such as duckdns.org. Please follow their instructions. Also make sure you use a DDoS protection service like Cloudflare if you're running at home.
* A Netlify account for frontend deployment
* A domain name. Namecheap is quite good.

#### Installation (Arch Linux)

* Become root: `sudo su -`
* Install caddy: `pacman -S caddy`
* Copy the `Caddyfile` to `/etc/caddy/Caddyfile` and replace `cutebox.cc` with your domain name
* Start, enable and reload the caddy server `sudo systemctl enable --now caddy && sudo systemctl reload caddy`
* Deploy [this](https://github.com/cuteboxcc/cutebox) repository to Netlify. This is the frontend which MUST reside in `web.YOURDOMAIN.TLD`. Replace `YOURDOMAIN.TLD` with your own domain name.
* Once deployment is finished on Netlify it should have detected a form. Configure the form so you can receive account requests.
* Clone the backend: `git clone https://github.com/cuteboxcc/cutebox-be /cutebox`
* Go into the cutebox-be directory: `cd /cutebox`
* Edit the configuration file so it fits with your domain name etc.
* Test if it runs: `./cutebox --config-path ./config.yaml server start`
* Open your browser on your domain name to see if you get redirected to the backend. If it fails check for any errors.
* Press Ctrl-C to quit the server for now

#### systemd configuration

In order to make your server run on bootup, additional configuration is necessary.

```
sudo useradd -r cutebox
sudo groupadd cutebox
sudo usermod -a -G cutebox cutebox
sudo chown -R cutebox:cutebox /cutebox
sudo cp /cutebox/cutebox.service /etc/systemd/system/
sudo systemctl enable --now cutebox
```

#### Creating a new user

You can use the cutebox binary to also create, confirm and promote your user account.

Run the following command to create a new account:

`./cutebox --config-path ./config.yaml admin account create --username some_username --email some_email@whatever.org --password 'SOME_PASSWORD'`

In the above command, replace `some_username` with your desired username, `some_email@whatever.org` with the email address you want to associate with your account, and `SOME_PASSWORD` with a secure password.

If you want your user to have admin rights, you can promote them using a similar command:

`./cutebox --config-path ./config.yaml admin account promote --username some_username`

That's it! Have fun with your new server.

## Admin CLI

Refer to the [GoToSocial](https://docs.gotosocial.org/en/latest/admin/cli/) documentation on how to use the admin CLI.

## Updating

To update to the latest cutebox-be release run `cd /cutebox && git pull`. The frontend receives automatic updates.
