Headless raspi-config
=========

Performs some `raspi-config nonint` operations using Ansible intended for minimal headless setup. It can also be used as a basis for multimedia setup.

Requirements
------------

Ansible Core version 2.8 or greater is mandatory. 

Only 32-bit Debian version 11 (Bullseye) Raspberry Pi OS has been tested and ran successfully. Tested on Raspberry Pi 3 & 4. Even worked on first-generation Raspberry Pi Model B.

It's assumed Raspberry Pi has been configured with [SSH server enabled](https://www.raspberrypi.com/documentation/computers/remote-access.html#enabling-the-server), and an existing [user account](https://www.raspberrypi.com/documentation/computers/remote-access.html#enabling-the-server) with passwordless sudo ability (which should already be granted by default) has been set up on it. Additionally, [key-based authentication](https://www.raspberrypi.com/documentation/computers/configuration.html#using-key-based-authentication) would also need to be configured for the user.

Role Variables
--------------

You can modify any of the following variables as you wish in the role's `defaults/main.yml`:

- `pi_hostname`: Customize your Pi's hostname. 
- `pi_timezone`: Customize your Pi's timezone. 
  - Please note you must use the appropriate timezone format. See the official [Debian documentation](https://wiki.debian.org/TimeZoneChanges) for more information on the timezone configuration.
- `pi_locale_setting`: Customize your Pi's locale's setting. 
  - See the official [Debian documentation](https://wiki.debian.org/Locale) for more information on the locale configuration. 
- `pi_keyboard_layout`: Customize your keyboard layout. 
  - See the official [Debian documentation](https://wiki.debian.org/Keyboard) for more inforamtion on the keyboard layout configuration.
- `pi_bootup_behavior`:	 Customize how you want your Pi to log in after bootup. 
  - This is [the setting](https://github.com/RPi-Distro/raspi-config/blob/408bde537671de6df2d9b91564e67132f98ffa71/raspi-config#L1395-L1398) that you'd see when you are interactively setting up the *Boot Options*.
- `pi_memory_split`: Configure the amount of memory you want to dedicate to the GPU.
  - See the official [Raspberry Pi documentation](https://www.raspberrypi.com/documentation/computers/config_txt.html#memory-options) for more information on GPU memory configuration. Here's the [source line](https://github.com/RPi-Distro/raspi-config/blob/408bde537671de6df2d9b91564e67132f98ffa71/raspi-config#L697) of the prompt from the interactive setup.


Example Playbook
----------------

First, you need to install the role, which you can via `ansible-galaxy` command.

`ansible-galaxy role install bashtheshell.headless_raspi_config`

You can verify if it's installed using `ansible-galaxy list` command.

Assuming you don't have an inventory file for your playbook but only a single Raspberry Pi with the IP address `192.168.1.10`, you can run the following playbook with the command:

`ansible-playbook --inventory "192.168.1.10," site.yml`

```yaml
---
### site.yml
- name: Main playbook is running...
  hosts: all
  remote_user: pi_user
  tasks:
    - name: Run the "bashtheshell/headless_raspi_config" role
      ansible.builtin.import_role:
        name: bashtheshell.headless_raspi_config
      vars:
        pi_hostname: "hotblueberrypie"
        pi_bootup_behavior: "B2"
```

You can also install the role from `git` this way:

`ansible-galaxy role install git+git@github.com:bashtheshell/headless-raspi-config-ansible.git`

Although, when you use `git`, the `name` value for `ansible.builtin.import_role` differs as it should be `headless-raspi-config-ansible`.

License
-------

[MIT LICENSE](./LICENSE)

Questions or Issues
------------------

Please submit [Issues](https://github.com/bashtheshell/headless-raspi-config-ansible/issues) or [Pull requests](https://github.com/bashtheshell/headless-raspi-config-ansible/pulls) and I'll get to them the best I can.
