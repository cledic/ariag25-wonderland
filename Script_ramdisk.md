
```
#!/bin/sh -e

if [ ! -e /tmp/ram ]; then
  mkdir /tmp/ram
fi

#
mount -t tmpfs -o size=32M,mode=0744 tmpfs /tmp/ram/
chmod 777 /tmp/ram/ -R
```