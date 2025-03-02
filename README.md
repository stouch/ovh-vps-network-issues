
Log and give solutions in OVH Debian/Ubuntu common issues when using vRack and/or Docker containers

# Debian 12 network : No internet access after initial setup

For some reason, after your initial setup and after you attached your OVH instance to a private network, you can loose access to internet.

For example: `ping google.com` timeouts.

## 1/2: Short solution (to check that this issue is going to help you)

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

## 2/2: Permanent solution 

First, check you got the issue:
```
ls -la /etc
```

You should see /etc/resolve.conf that links to :
```
lrwxrwxrwx  1 root root      32 Oct  4 04:35 resolv.conf -> /run/systemd/resolve/resolv.conf
```
which is not correct.

**If have ANY other symlink, don't use the following method. If you have this symlink, you can go further:**

Permanent solution:

a.
```bash
sudo nano /run/systemd/resolve/stub-resolv.conf
```
```conf
nameserver 127.0.0.53
options edns0 trust-ad
search .
```

b.
```bash
sudo mv /etc/resolv.conf /etc/resolve.conf.backup
sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf

sudo netplan try
sudo netplan apply
sudo systemctl restart systemd-resolved
```
