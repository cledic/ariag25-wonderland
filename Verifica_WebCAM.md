
```
#include "highgui.h"
#include <stdio.h>

int main( int argc, char** argv ) {

    CvCapture* capture = cvCaptureFromCAM( CV_CAP_ANY );
    IplImage* frame;

    frame = cvQueryFrame( capture );
    if( frame ) {
        printf("Camera  OK\n");
        printf("\th: %d, w: %d\n", frame->height, frame->width);
    } else {
        printf("Camera  Bad\n");
    }
    cvReleaseCapture( &capture );
}
```