# bash-cheatsheet

## word caount without space
A file
`tr -d '[:space:] ' < myfile.json | wc`

For all files
`for f in ./*; do cat $f | tr -d '[:space:] ' | wc; done`
