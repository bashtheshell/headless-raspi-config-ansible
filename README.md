Headless raspi-config
=========

Performs some `raspi-config nonint` operations using Ansible intended for minimal headless setup. It can also be used as a basis for multimedia setup.

Requirements
------------

Ansible Core version 2.13.9 or greater is mandatory [according to Ansible](https://github.com/ansible/ansible-hub-ui/blob/a71e7de445c05f4f193650c5daa867df255fa0b0/src/containers/landing/landing-page.tsx#L75). 

Only 32-bit Debian version 12 (Bookworm) Raspberry Pi OS has been tested and ran successfully. Tested on Raspberry Pi 3 & 4. Even worked on first-generation Raspberry Pi Model B, albeit ran quite slowly. Pi 5 hasn't been tested yet at the time of this writing.

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
- ~~`pi_memory_split`~~: **This is [no longer supported](https://github.com/RPi-Distro/raspi-config/commit/1089abb821ee0f32c8451fcd62b9df88f047ea01), starting with Bookworm.** Please see [v1.0 release](https://github.com/bashtheshell/headless-raspi-config-ansible/tree/87186c4a4838527c579531967547774d31cb767d) if you want to use this option on an older Debian version.


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


If you prefer to test only the role quickly without the hassle of creating a playbook after `git` cloning the repo, you can just use the ad-hoc `ansible` command instead which is the equivalent of the above playbook:

```yaml
ansible all -i "192.168.1.10," -m import_role -a role='./headless-raspi-config-ansible' -u pi_user -e pi_hostname="hotblueberrypie" -e pi_bootup_behavior="B2"
```


Run Time
--------

Below are the average playbook run times using the sample playbook above (*YMMV*):

- Raspberry Pi 1: `53:28.67 total`
- Raspberry Pi 3: `13:02.19 total`
- Raspberry Pi 4: `9:14.41 total`


License
-------

[MIT LICENSE](./LICENSE)


Questions or Issues
------------------

Please submit [Issues](https://github.com/bashtheshell/headless-raspi-config-ansible/issues) or [Pull requests](https://github.com/bashtheshell/headless-raspi-config-ansible/pulls) and I'll get to them the best I can.
