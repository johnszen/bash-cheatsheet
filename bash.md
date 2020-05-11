# bash-cheatsheet

### copy from remote machine
```
#scp <remote-machine>:<full path to file> <local path>
scp a.b.c:/mnt/jenkins/conf* .

# copu directory
scp -r <remote-machine>:/mnt/jenkins <destination-dir>
```

### word count without space
A file

`tr -d '[:space:] ' < myfile.json | wc`

All files in a directory

`for f in ./*; do cat $f | tr -d '[:space:] ' | wc; done`

### top 5 processes with highest memory usage (by ps)
```
ps aux | awk '{print $2, $5, $11}' | sort -k2rn | head -n 5
```

### top 10 ip invalid login
```
grep -oE "Invalid user[[:space:]]+[a-zA-Z0-9]+[[:space:]]+from ([0-9]+\.){3,3}[0-9]" /var/log/secure | awk '{print $5}' | sort | uniq -c | sort -s -k1,1nr | head -10
```

