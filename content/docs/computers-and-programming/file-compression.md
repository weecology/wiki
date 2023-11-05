---
title: "File Compression Notes"
type: book
summary: " "
---

## Fixing a corrupted zip file

If you try to open a zip file and it won't unzip you can often fix it by rezipping the file ([source](https://superuser.com/questions/23290/terminal-tool-linux-for-repair-corrupted-zip-files)).

First, try:

```sh
zip -F corrupted.zip --out fixed.zip
```

If that doesn't work try:

```sh
zip -FF corrupted.zip --out fixed.zip
```

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
