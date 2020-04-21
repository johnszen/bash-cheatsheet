# bash-cheatsheet

## word caount without space
A file

`tr -d '[:space:] ' < myfile.json | wc`

All files in a directory

`for f in ./*; do cat $f | tr -d '[:space:] ' | wc; done`
