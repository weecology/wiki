---
title: "Globus for file transfer"
summary: >
  How to transfer large files between your computer and the HiPerGator
---

[Globus](https://app.globus.org) is a useful file manager for transferring files between your computer and the HiPerGator, or from place to place on the HiPerGator.

[Here is the HiPerGator guide to Globus.](https://help.rc.ufl.edu/doc/Globus) It may be more up to date.

Briefly, to use Globus, you need to create an account with your UF login. You may need to get authorization to be added to the UF research computing access. Once you have access, you can use the online Globus interface to set up file transfers from different locations (called "endpoints") on the HiPerGator.

To transfer files to and from your computer, you need to install the Globus Connect Personal account on your computer and set it up as an "endpoint". Then you can also set up transfers to and from your computer.


### Downloading Files from HiPerGator via Globus

1. Go to [globus.org](https://globus.org) and log in with your GatorLink credentials,
   selecting **University of Florida** as the authentication provider.
2. Type or paste **`UFRC HiPerGator`** into the collection search box.
3. Type or paste the path to your directory (e.g. `/blue/ewhite` or `/orange/ewhite`)
   into the path box. You may be prompted to grant the **Globus Application** permission
   to access your files.
4. Browse to the file you want, select it, and click **Download** —
   the file will appear in your laptop's downloads folder.

---

If you want to zip a specific folder then run:

```bash
zip -r folder_name.zip folder_name/
```

If you're downloading a results folder with many files, zip it on HiPerGator
first. On HiPerGator `cd` into the parent directory where the folder of interest is and run:

```bash
zip -r "$(ls -Art | tail -n 1).zip" "$(ls -Art | tail -n 1)"
```

This zips the most recently created folder, which will then appear in Globus ready
for download. 


