```
find / -user root -perm -4000 -print 2>/dev/null
find / -perm -u=s -type f 2>/dev/null 
find / -user root -perm -4000 -exec ls -ldb {} \;
```

