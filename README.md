# Charge-Limit

Enforces battery charges threshold via /sys/class/power_supply/BAT0/charge_control_end_threshold at boot.
 
## Description

The battery charge threshold set via /sys/class/power_supply/BAT0/charge_control_end_threshold is not persistent at boot on my laptop, a .service file applies the value stored in /usr/local/share/charge-limit to /sys during boot, you can also toggle between 80 and 100 using charge-limit-toggle script. tmpfiles is used to create the file with 80% as default value and a waybar module is present to display the current charge threshold and toggle between 80 and 100.

## Getting Started

### Dependencies

* Running linux distribution with systemd
* A laptop that exposes the charge threshold via /sys filesystem

### Installing

* Create the chlimit group and add you user to it
```
sudo groupadd -U <user> chlimit
```

* Edit the sudoers file to that members of the chlimit group can restart the service
```
sudo visudo -f /etc/sudoers.d/99-chlimit
%chlimit        ALL=(ALL)	NOPASSWD: /bin/systemctl start charge-limit.service
```

* Copy the tmpfile to /etc/tmpfiles.d/
```
sudo cp tmpfiles.d/charge-limit.conf /etc/tmpfiles.d/charge-limit.conf
sudo systemd-tmpfiles --create
```

* Copy the .service via to /etc/systemd/system and set it to start at boot:
```
sudo cp systemd/charge-limit.service /etc/systemd/system/
sudo systemctl start --now charge-limit.service 
```

* Copy the toggle script to /usr/loca/bin/
```
sudo cp charge-limit-toggle /usr/local/bin/
```

* If you use wayland you can copy the waybar module to your ~/.config/waybar/modules/
```
cp waybar-modules/charge-limit.json ~/.config/waybar/modules/
```

### Executing program

* You can use the script in to toggle the threshold between 80 and 100. The script will start again the service so the limit get set to /sys filesystem
```
charge-limit-toggle
```
* You can simply click on the waybar module to launch the script again.

## License

This project is licensed under the MIT License - see the LICENSE.md file for details
