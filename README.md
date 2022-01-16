# No NAT No Cry

## Story

Your professor wants to create a webpage on his computer to display basic information about some running services on his machine. He almost managed to figure this out by himself, but he set his computer on fire in the end.

He had his webpage ready, he could figure out how to install a webserver to serve it, found out how to display runtime data about services on his machine, but he couldn’t figure out how to make his website reachable over the school's local network or the Internet.

You step in to educate the prof how this should be done using virtualization and whatnot.

So, help the prof create a VM to host his website, figure out if he's behind a Carrier-grade NAT, and make his website available over the Internet (but at least on the local network)!

## What are you going to learn?

* Understand how the default NAT networking works in VirtualBox
* Understand what a port forward and NAT is
* Understand the Unix service concept (daemons, background services, automatic start on boot, etc.)
* How to list, start, stop and check service status
* Enable and disable services, make them start automatically on boot
* Understand the basic concept of "package management"
* Install packages with `apt-get`
* Learn how to create a new user on a Linux system
* Learn about Apache HTTP web server
* How to determine if behind a Carrier-grade NAT
* Understand what Carrier-grade NAT or CGN is (also known as Large-scale NAT, LSN, double NAT, etc.)
* Understand the reason for Carrier-grade NAT (CGN) in relation to IPv4 exhaustion
* Understand the conceptual difference between a public IP and a WAN IP

## Tasks

1. Start and configure a VM with a NAT network adapter.
    - Started a VM loaded and configured with an SSH server (either an existing one or a fresh one using [this image](https://github.com/CodecoolBase/short-admin-vms/releases/latest/download/ubuntu-18.04-base.ova))
    - The VM's Network settings are configured with a single _NAT_ adapter
    - Port 22 is forwarded between the host and the guest in the VM's NAT network adapter's settings
    - The guest VM's OS is reachable via `ssh` using the `ubuntu` user

2. Install an Apache web-server on the guest VM and make the service start on boot automatically. Also, make sure that the `userdir` module is installed and enabled.
    - The Apache web-server (also known as `apache2` or `httpd`) is intalled on the system
    - The Apache server service is in a running state
    - The Apache server service is configured to start on boot
    - The `userdir` Apache module is installed, enabled and loaded by the web-server

3. Create a new user on the system called `professor` which will be used by your Professor to do his thing.
    - A new user called `professor` with the password `professor` is added to the system
    - The user has a home directory at `/home/professor`
    - The user has folder called `public_html` directly inside his home directory to host a personal website in

4. Help create the prof a static HTML page with the information he requires and make that accessible on your local network.
    - An `index.html` file is present in the `public_html` directory using this structure

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="ie=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Services</title>
  </head>
  <body>
  <h1>Services</h1>
  <pre>...</pre>
  <pre>...</pre>
  <pre>...</pre>
  </body>
</html>
```
    - In the HTML file the `<pre>` tags contain the status output of the following services as reported by `systemctl`: VirtualBox Guest Utils, Apache HTTP Server, Uncomplicated Firewall
    - Website is reachable from inside the guest OS via `http://localhost/~professor`, you can use `curl http://localhost/~professor` to test it
    - Website reachable from host OS via `http://localhost:<port>/~professor`, where port is chosen from the ["user port range"](project/curriculum/materials/pages/networks/ports.md)
    - Whenever the VM is rebooted the website is reachable automatically without any manual configuration

5. If you're not behind a CGN you can allow the whole Internet to access your website. Let's try and do that, but again, only if you're _not_ behind a CGN, otherwise this won't work.
    - Website reachable from the public Internet via `http://<public ip>:<port>/~professor`, where port is chosen from the ["user port range"](project/curriculum/materials/pages/networks/ports.md)

## General requirements

None

## Hints

- Installing software on a Linux OS is primarily done via package managers, not by downloading archives and installing/configuring individual tools
- If port forwarding doesn't seem to work out for you make sure that no other service or VM is using the problematic port in question on your host machine
- If you configure port forwardings on your local, home router make sure to clean them up after you're done for maximum security
- You can ask a friend or somebody else to check out if your website is accessible locally/globally or not
- Here's how to determine if you're behind a Carrier-grade NAT or not
  1. Check what's your external, public IP (e.g. Google _what's my IP?_)
  1. Check your _WAN IP_ on your home router's adminstrator page
  1. If their are the same you're _not_ behind a CGN
  1. If your WAN IP falls in the [IP address range of 100.64.0.1 to 100.127.255.254](http://subnet.im/100.64.0.0/10) then you're behind a CGN
  1. Otherwise ... hard to tell, let's hope it won't come to this ;)
- Should your ISP use a CGN you can try and ask them to assign your connection a public/static IP address (they might comply, but your mileage may vary and it may not be free)
- **When you are reading background materials:**
  - [Networking modes in VirtualBox](https://www.youtube.com/watch?v=QO_Tv02ND-k)
    - All available types of networking mode are explained, focus on the simple "NAT" mode, not the "NAT network" mode
  - [Apache `mods-enabled` directory](https://geek-university.com/apache/mods-enabled-directory/)
    - Enabling/disabling sites works similarly, but you don’t need to deal with this here
  - [Discussion about Carrier-grade NAT](https://www.youtube.com/watch?v=9tzy_6itUYM)
    - NAT444 is just a different name for Carrier-grade NAT

## Background materials

- <i class="far fa-exclamation"></i> [Ports](project/curriculum/materials/pages/networks/ports.md)
- <i class="far fa-exclamation"></i> [Networking modes in VirtualBox](https://www.youtube.com/watch?v=QO_Tv02ND-k)
- <i class="far fa-exclamation"></i> [Port Forwarding Explained](https://www.youtube.com/watch?v=2G1ueMDgwxw)
- [Port forwarding in VirtualBox](https://www.simplified.guide/virtualbox/port-forwarding)
- [Explanation about NAT and port forwarding](https://www.youtube.com/watch?v=92b-jjBURkw)
- <i class="far fa-exclamation"></i> [How to create web pages in home directory and have the webserver serve them in the browser?](https://unix.stackexchange.com/a/30962)
- <i class="far fa-exclamation"></i> [Apache `mods-enabled` directory](https://geek-university.com/apache/mods-enabled-directory/)
- [Using `systemctl` to manage services](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)
- [User and group management in Linux](https://www.pluralsight.com/guides/user-and-group-management-linux)
- [Why is my Router's WAN IP different from public IP?](https://networkengineering.stackexchange.com/questions/20063/why-is-my-routers-wan-ip-different-from-public-ip)
- [Discussion about Carrier-grade NAT](https://www.youtube.com/watch?v=9tzy_6itUYM)
