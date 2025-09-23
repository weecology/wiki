---
title: "Accessing the T-drive"
summary: " "
---

- If off campus, turn on the virtual private network (VPN) before connecting using the instructions here: https://vpn.ufl.edu/
- Follow the instructions for: 1) [Windows](https://wec.ifas.ufl.edu/resources/it--computer-support/mapping-your-t-and-u-drives/) or 2) [macOS](https://wec.ifas.ufl.edu/resources/it--computer-support/mapping-your-t-and-u-drives-mac/)
- On Linux use the same basic approach described in the Windows/macOS instructions, but enter `UFAD` for the `WORKGROUP`
- Lab materials are in the `lab-white-ernest` folder
- Our public file serving folder is `Weecology`

## Linux

Assuming you're running Ubuntu. Install the cifs-utils package, create a folder called /media/T for a mount point, then run the mount command, replacing `<your-gatorlink>` with your UF gatorlink (the part of your UF email address before the `@` and `<your-local-username>` with your username on the Linux machine you are performing the mount from; you can check this with `whoami`). If you haven't run `sudo` recently you will be prompted for your local password. You will (then) be prompted for UF your password.

```
sudo apt-get install cifs-utils
sudo mkdir /media/T
sudo mount -t cifs LINK_FROM_INSTRUCTIONS_ABOVE_WITHOUT_SMB_PART /media/T/ -o username=<your-gatorlink>,uid=<your-local-username>,gid=<preferred-local-group>
```

The `,gid=<preferred-local-group>` can be left out if no group is needed, but on shared systems (like Serenity) we typically want to include a group so everyone can access the share.
The full command, including the `LINK_FROM_INSTRUCTIONS_ABOVE_WITHOUT_SMB_PART`, is available for weecology folks as a pinned post on the Serenity and HiPerGator Slack channels.
