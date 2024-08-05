---
title: "File Compression Notes"
summary: " "
---

## Efficient large volume (de)compression

When archiving large volumes of data using parallel and highly efficient algorithms can be useful.
We most commonly do this when archiving old projects on the HPC.

On Linux (and our HPC) one of the easy ways to do this is with tar with zstd compression.

```sh
tar --use-compress-program=zstd -cvf my_archive.tar.zst /path/to/archive
```

If you need to pass arguments to zstd they can be included in quotes, `'zstd -v'`.

To uncompress these archives:

```sh
tar --use-compress-program=unzstd -xvf my_archive.tar.zst
```

## Ignoring failed reads using tar

When archiving files with tar the archive will fail if any file cannot be read by the account doing the archiving.
This is a common occurrence we archiving on the HPC and the files are often (but not always) hidden files that don't need to be archived (but definitely check to make sure).
You can ignored these failed reads using the `--ignore-failed-read` flag.

```sh
tar --ignore-failed-read -cvf my_archive.tar.zst /path/to/archive
```

## Fixing a corrupted zip file

### Using zip

If you try to open a zip file and it won't unzip you can often fix it by rezipping the file ([source](https://superuser.com/questions/23290/terminal-tool-linux-for-repair-corrupted-zip-files)).

First, try:

```sh
zip -F corrupted.zip --out fixed.zip
```

If that doesn't work try:

```sh
zip -FF corrupted.zip --out fixed.zip
```

If you receive an error message like:

> zip error: Entry too big to split, read, or write (Poor compression resulted in unexpectedly large entry - try -fz)

then:

1. Make sure you have at least version 3.0 of `zip`
2. Try adding `-fz` to the command

```sh
zip -FF -fz corrupted.zip --out fixed.zip
```

### Using p7zip

If none of this works try [p7zip](https://7-zip.org/), which can be installed using conda.

```sh
conda create -n p7zip python=3
conda activate p7zip
conda install -c bioconda p7zip
```

This version is pretty out of date, but much less out of date than the one in the HiperGator module system.
The one in the HiperGator module is too old to solve the problems we've seen.

```sh
7za x corrupted.zip
```

Note that this will decompress into the current working directory not into `./corrupted/`

## Increasing compression

There is a tradeoff between how long it takes to compress something and how much smaller gets.
When using `zip` this is controlled by a numeric argument ranging from 1 (faster) to 9 (smaller).
So, if you're archiving large objects try using `zip -9`.

If you have a bunch of already zipped files you can recompress them using the following bash loop:

```sh
for f in *.zip
do
  mkdir ${f%.*}
  unzip -d ${f%.zip} $f
  rm $f
  rm ${f%.*}/${f%.*}.csv
  zip -r -9 $f ${f%.zip}
done
```
