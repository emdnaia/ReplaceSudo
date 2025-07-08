#a ) Replace Sudo with OpenDoas

- The package is called `opendoas` and is present at almost any major Linux distro!
- Steps are 0 ) adding the user to the wheel grp (should be default most cases), 1) adding that grp and allowing longer persistence before timeouts & 2-3) disabling the sudo binary
```
# 0 add to wheel grp
sudo usermod -aG wheel myuser

# 1 change /etc/doas.conf to only allow wheel users
permit persist :wheel

# 2 disable rather than remove the sudo binary
sudo chmod 0 /usr/bin/sudo

# 3 make it sticky to prevent changes
doas chattr +i /usr/bin/sudo
```

#b) In case you wanna reenable sudo some day
```
doas chmod 4711 /usr/bin/sudo
doas chattr -i /usr/bin/sudo
```

#c) NixOS version

```
# service
  security.doas.enable = true;
# config 
  security.doas.extraRules = [{
    users = [ "myuser" ];
    keepEnv = true; # optional
    persist = true; # optional
  }];

#pam
  security.pam.services = {
  # other
    doas.u2fAuth    = true;    
  };

``` 
