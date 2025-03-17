# Linux FreeRADIUS Installation

This is a walkthrough on how to install and configure freeRADIUS.

## Gaining Sudo Permissions

Gain root account permissions in the terminal.

```bash
su -l
# You'll have to enter the password for the root account to gain permissions.
```

Add the user you want to give sudo permissions to install freeradius. Then, install freeRADIUS.

```bash
adduser <user> sudo
sudo apt install freeradius
# Confirm installation of freeRADIUS
```

## Make the Device Remain Active

```bash
cd /etc/systemd
sudo nano logind.conf
```

If using a laptop, set `HandleLidSwitch` to `ignore` and uncomment the line.\
There are other settings you can configure; configure as needed.

## Configuring a FreeRADIUS Client

Go to the directory that stores client configuration and start editing.

```bash
cd /etc/system/freeradius/3.0
sudo nano clients.conf
# Note: depending on the version of freeRADIUS or the distro of linux,
# freeradius configuration files could be stored in other locations.
```

Add a client to `clients.conf`. For example:

```conf
client AP {
        ipaddr = 10.0.0.3
        require_message_authenticator = no
        secret = password
        nas_type = "other"
        proto = "*"
}
```

## Configuring FreeRADIUS Users

```bash
cd /etc/system/freeradius/3.0/mods-config/files
sudo nano authorize
```

Add some users to `authorize`. For example:

```conf
admin   Cleartext-Password := "password"
        Reply-Message := "Hello, %{User-Name}"

username  Cleartext-Password := "password"
        Reply-Message := "Hello, %{User-Name}"
```

## Running FreeRADIUS

```bash
sudo systemctl start freeradius
```

Here are some other commands that may be useful:

```bash
sudo systemctl status freeradius
sudo systemctl restart freeradius
```

You're done! \\(^▽^)/
