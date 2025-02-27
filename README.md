
Log and give solutions in OVH Debian/Ubuntu common issues when using vRack and/or Docker containers

# Debian 12 network : No internet access after initial setup

For some reason, after your initial setup and after you attached your OVH instance to a private network, you can loose access to internet.

For example: `ping google.com` timeouts.

## Solution

```bash
sudo nano /etc/resolv.conf
```

The file might contains something like that: 

```conf
nameserver 213.186.33.99
nameserver 1.1.1.1
nameserver 1.0.0.1
# Too many DNS servers configured, the following entries may be ignored.
nameserver 213.186.33.99
nameserver 1.1.1.1
nameserver 1.0.0.1
nameserver 213.186.33.99
nameserver 1.1.1.1
nameserver 1.0.0.1
search .
```

Just remove all of this and change it to:
```conf
nameserver 127.0.0.53
options edns0 trust-ad
search .
```
