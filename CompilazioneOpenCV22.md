# Come compilare OpenCV 2.2 sulla board AriaG25 #

Di seguito riporto i dettagli per compilare le OpenCV 2.2 direttamente sulla board AriaG25.
  * Sync dell'ora da Internet
```
apt-get install rdate
rdate 193.204.114.233
```

  * Aggiornamento
```
apt-get update
apt-get upgrade
```

  * Ho installato i seguenti package
```
apt-get install build-essential
apt-get install cmake
apt-get install pkg-config
apt-get install libpng12-0 libpng12-dev libpng++-dev libpng3
apt-get install libpnglite-dev libpngwriter0-dev libpngwriter0c2
apt-get install zlib1g-dbg zlib1g zlib1g-dev
apt-get install libjasper-dev libjasper-runtime libjasper1
apt-get install pngtools libtiff4-dev libtiff4 libtiffxx0c2 libtiff-tools
apt-get install libjpeg8 libjpeg8-dev libjpeg8-dbg
apt-get install ffmpeg libavcodec-dev libavcodec52 libavformat52 libavformat-dev
apt-get install libgstreamer0.10-0-dbg libgstreamer0.10-0  libgstreamer0.10-dev
apt-get install libxine1-ffmpeg  libxine-dev libxine1-bin
apt-get install libunicap2 libunicap2-dev
apt-get install libdc1394-22-dev libdc1394-22 libdc1394-utils
apt-get install swig
apt-get install libv4l-0 libv4l-dev
#
apt-get install libjpeg-progs libjpeg-dev
apt-get install libgstreamer-plugins-base0.10-dev
#
apt-get install cmake-curses-gui
#
apt-get install libswscale-dev

```

  * A questo punto creo una directory dove scompattare le OpenCV.
```
mkdir opencv
cd opencv
```

  * Scaricare OpenCV-2.2.0.tar.bz2 da
http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.2/

  * Una volta fatto scompattare eseguendo:
```
tar -xjvf OpenCV-2.2.0.tar.bz2
```
  * Commetare la riga 52 nel file: OpenCV-2.2.0/modules/features2d/src/sift.cpp:
```
#ifdef arm
// #define ARM_NO_SIFT
#endif
```

  * Creare la directory di lavoro:
```
mkdir build
cd build
```

  * Eseguire prima
```
cmake ../OpenCV-2.2.0/
```

  * e poi:
```
ccmake .
```

  * Dalla finesrta testuale mettere in OFF i seguenti moduli:
```
    BUILD_NEW_PYTHON_SUPPORT
    BUILD_TESTS
    WITH_1394
    WITH_CUDA
    WITH_EIGEN2
    WITH_FFMPEG
    WITH_GSTREAMER
    WITH_GTK
    WITH_JASPER
    WITH_JPEG
    WITH_OPENEXR
    WITH_PNG
    WITH_PVAPI
    WITH_QT
    WITH_QT_OPENGL
    WITH_TBB
    WITH_TIFF
    WITH_UNICAP
    WITH_V4L
    WITH_XINE 
```

  * premere 'c' e poi 'g' per, rispettivamente, configurare e generare i makefile
  * eseguire poi il make
```
make
```

  * Questo serve per verificare se tutto si compila senza problemi.
  * Se così è, eseguire nuovamente:
```
ccmake .
```

  * dalla finestra impostare ad ON
```
  WITH_JPEG
  WITH_PNG
  WITH_V4L
```

  * premere 'c' e poi 'g' e poi eseguire:
```
make
```

  * se tutto è OK eseguire:
```
make install
```

  * Editare/creare il file:
```
vi /etc/ld.so.conf.d/opencv.conf
```

  * ed aggiungete la riga:
```
/usr/local/lib
```

  * salvate e poi eseguite:
```
ldconfig
```

  * A questo punto è possibile iniziare ad usare OpenCV 2.2

  * Riferimenti:
http://processors.wiki.ti.com/index.php/Building_OpenCV_for_ARM_Cortex-A8

http://opencv.willowgarage.com/wiki/InstallGuide%20:%20Debian