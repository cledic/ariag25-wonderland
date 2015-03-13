
```
http://<IP_ARIAG25>/stream/
```

  * Questo script viene eseguito dal plugin "output\_file.so" ed esegue il Face Detection sull'immagine che gli viene passata.
```
debarm:~/mjpg-streamer# cat img2opencv_fd.sh
#!/bin/bash

# Face Detection

# Questo script viene eseguito da mjpgstream e gli viene passato come argomento il
# nome del file .JPG che ha creato.

# Il comando detectface2 apre il file e evidenzia nel file di destinazione image.png
# il volto trovato.
/root/mjpg-streamer/detectface2 $1 /var/www/stream/image.png

# rimuovo il file .JPG creato da mjpgsrteam
rm $1

exit 0
```

  * Questo è lo script con cui avvio mjpg-streamer. Lo script crea una ramdisk che monta sotto la dir /var/www/stream. Qui verrà salvata l'immagine acquisita da mjpg-streamer ed elaborata successivamente.

```
debarm:~/mjpg-streamer# cat mjpg2webcv_fd.sh
#!/bin/sh -e

if [ ! -e /var/www/stream ]; then
  echo Costruisco la dir /var/www/stream
  mkdir /var/www/stream
fi

#
mount -t tmpfs -o size=32M,mode=0744 tmpfs /var/www/stream
chmod 777 /var/www/stream -R

echo RamDisk creata e montata sotto /var/www/stream

#
cd /root/mjpg-streamer
cp ./index.html /var/www/stream

echo Copiato index.html in /var/www/stream

#
./mjpg_streamer -i "./input_uvc.so -r 160x120 -f 4" -o "./output_file.so -f /var/www/stream -c ./img2opencv_fd.sh"

```

  * Di seguito il file index.html
```
<html>
<meta http-equiv="refresh" content="5" >
<body>
<img src="image.png" alt="image">
</body>
</html>
```