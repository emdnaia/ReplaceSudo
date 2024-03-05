# Replace Sudo with OpenDoas

- The package is called `opendoas` and is present at almost any major Linux distro atm
- Steps are 0 ) adding the user to the wheel grp (should be default most cases), 1) allowing a bit of persistence for that group user with longer timeouts & allowing that wheel group and 2-3) disabling the sudo binary plus a way to revert back
```
# 0 add to wheel grp
sudo usermod -aG wheel myuser

# 1 change /etc/doas.conf to only allow wheel users
permit persist :wheel

# 2 disable rather than remove the sudo binary
sudo chmod 0 /usr/bin/sudo

# 3 make it sticky to prevent changes
doas chattr +i /usr/bin/sudo

# In case you wanna revert it:
doas chmod 4711 /usr/bin/sudo
doas chattr -i /usr/bin/sudo
```
