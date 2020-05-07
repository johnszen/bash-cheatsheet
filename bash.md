# bash-cheatsheet

## copy from remote machine
```
#scp <remote-machine>:<full path to file> <local path>
scp a.b.c:/mnt/jenkins/conf* .

# copu directory
scp -r <remote-machine>:/mnt/jenkins <destination-dir>
```

## word caount without space
A file

`tr -d '[:space:] ' < myfile.json | wc`

All files in a directory

`for f in ./*; do cat $f | tr -d '[:space:] ' | wc; done`

# volume
```
lsblk

df
```
