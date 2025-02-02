# Changes made in this fork
- Toggle a given wireguard interface up or down using `pkexec` by clicking the xfce4-panel icon
- Ping the VPN server to test connectivity instead of `ifconfig.me` for improved privacy.
  (At the cost of not seeing the IP address when hovering over the xfce4-panel icon)
- 16x16 pixel icons added.

# wireguard-monitor

[![Join the chat at https://gitter.im/VoidVolker/wireguard-monitor](https://badges.gitter.im/VoidVolker/wireguard-monitor.svg)](https://gitter.im/VoidVolker/wireguard-monitor?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Simple bash script for monitoring current IP and WireGuard status in Xfce tray.

# Requirements

* Xfce desktop
* Installed genmon-plugin: https://docs.xfce.org/panel-plugins/xfce4-genmon-plugin
```bash
sudo apt install xfce4-genmon-plugin
```
* pkexec (if a wireguard interface is specified using the `-w` flag
```bash
sudo apt install policykit-1
```

# Features

* Colored tray icon:
green - all ok
red - wireguard is not working
gray - offline or network error
* WireGuard status parsing
* Real IP check and WireGuard endpoind compare
* Periodical run
* Immediatly run by icon click

# Install

Go to your favorite directory for scripts:
```bash
mkdir ./scripts
cd ./scripts
```

Clone repository:
```bash
git clone https://github.com/william-stacken/wireguard-monitor
```

Allow `wg show` command to run without password:
```bash
sudo visudo
```

Add next line to sudoers file:
```
username ALL = NOPASSWD: /usr/bin/wg show
```
Where `username` is your username. This command is used for collecting wireguard status.

Add genmon widget
```bash
xfce4-panel --add genmon
```

Now you need to get widget name. To get this name, go to the panel properties screen and on the Items tab, hover your mouse over the genmon plugin to get it's internal name.

You will get: ```genmon-X```. Where `X` is plugin number. Example: `genmon-10`. Use this number as first argument for script. Plugin name is used for plugin refresh via icon click.

Go to genmon plugin properties and add next settings:

* Command: `./scripts/wireguard-monitor/wireguard-monitor -n X`, where `X` is plugin number.
* Period: `60`

Or alternatively, to toggle wireguard interface `wg0` up or down when the icon is clicked
(requires the user to enter their sudoers password using `pkexec`):

* Command: `./scripts/wireguard-monitor/wireguard-monitor -w wg0 -n X`, where `X` is plugin number.
* Period: `60`

# Command line arguments

| Short name | Long name | Default value | Description |
| --- | --- | --- | --- |
| -n | --name       | -                 | Widget/plugin name |
| -w | --interface       | -                 | An interface to bring up or down when the icon is clicked |
| -i | --icon       | 32                | Icon size. Founded sizes: 16 24 32 48 64 |
| -f | --face       | 'JetBrains Mono'  | Font family |
| -e | --error      | ff4400            | Error color |
| -p | --ip         | 9dffa8            | Ip color |
| -n | --endpoint   | 707eff            | Endpoint color |
| -h | --handshake  | a164e0            | Handshake color |
| -t | --transfer   | f1e552            | Transfer color |
| -c | --command    | poll vpn status   | Custom command to run on click |
