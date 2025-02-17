---
title: "Accessing the T-drive"
summary: " "
---

* If off campus, turn on the virtual private network (VPN) before connecting using the instructions here: https://vpn.ufl.edu/
* Follow the instructions for: 1) [Windows](https://wec.ifas.ufl.edu/resources/it--computer-support/mapping-your-t-and-u-drives/) or 2) [macOS](https://wec.ifas.ufl.edu/resources/it--computer-support/mapping-your-t-and-u-drives-mac/)
* On Linux use the same basic approach described in the Windows/macOS instructions, but enter `UFAD` for the `WORKGROUP`
* Lab materials are in the `lab-white-ernest` folder
* Our public file serving folder is `Weecology`

## Linux

Assuming you're running Ubuntu. Install the cifs-utils package, create a folder called /media/T for a mount point, then run the mount command, replacing `<your-ufid>` with your ufid (the same one used for email). You will be prompted for your password.

    sudo apt-get install cifs-utils
    sudo mkdir /media/T
    sudo mount -t cifs LINK_FROM_INSTRUCTIONS_ABOVE_WITHOUT_SMB_PART /media/T/ -o username=<your-gatorlink>
    
(Yes, it would be nice if the LINK_FROM_INSTRUCTIONS_ABOVE_WITHOUT_SMB_PART could be provided directly, but apparently UFIT is a 'security through obscurity' shop ¯\_(ツ)_/¯).