
```
#include <opencv/cvaux.h>
#include <opencv/highgui.h>
#include <opencv/cxcore.h>
#include <stdio.h>

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <assert.h>
#include <math.h>
#include <float.h>
#include <limits.h>
#include <time.h>
#include <ctype.h>

int trovata=0;

int main(int argc, char* argv[])
{

    IplImage* frame = 0;

    if (argc > 1) {
	frame = cvLoadImage(argv[1], CV_LOAD_IMAGE_COLOR);
    } else {
	printf("Errore nell'aprire il file\n");
	exit(1);
    }

    if(frame==NULL ) {
	printf("unable to load the frame\n");
	exit(2);
    }

    // Default capture size - 640x480
    CvSize size = cvSize(frame->height,frame->width);

    // Detect a red ball
    CvScalar hsv_min = cvScalar(150, 84, 130, 0);
    CvScalar hsv_max = cvScalar(358, 256, 255, 0);

    IplImage *  hsv_frame    = cvCreateImage(size, IPL_DEPTH_8U, 3);
    IplImage * thresholded   = cvCreateImage(size, IPL_DEPTH_8U, 1);

    printf("CreateImage Done\n");


    // Covert color space to HSV as it is much easier to filter colors in the HSV color-space.
    cvCvtColor(frame, hsv_frame, CV_BGR2HSV);
    printf("OK color space\n");
   
    // Filter out colors which are out of range.
    cvInRangeS(hsv_frame, hsv_min, hsv_max, thresholded);
    printf("OK color range\n");

    // Memory for hough circles
    CvMemStorage* storage = cvCreateMemStorage(0);
    // hough detector works better with some smoothing of the image
    cvSmooth( thresholded, thresholded, CV_GAUSSIAN, 9, 9 );
    CvSeq* circles = cvHoughCircles(thresholded, storage, CV_HOUGH_GRADIENT, 2,thresholded->height/4, 100, 50, 10, 400);

    printf("Detection Done\n");

    if ( circles->total) {
	trovata++;
	for (int i = 0; i < circles->total; i++)
	{
		// 
		float* p = (float*)cvGetSeqElem( circles, i );
		printf("\tBall! [%d][%d] x=%f y=%f r=%f\n\r",trovata,i,p[0],p[1],p[2] );
	        cvCircle( frame, cvPoint(cvRound(p[0]),cvRound(p[1])), 3, CV_RGB(0,255,0), -1, 8, 0 );
	        cvCircle( frame, cvPoint(cvRound(p[0]),cvRound(p[1])), cvRound(p[2]), CV_RGB(255,0,0), 3, 8, 0 );
	}

	#	
	if ( !cvSaveImage( "resobj.jpg", frame)) {
		printf("Errore nel salvare resobj.png\n");
	} else {
		printf("OK salvataggio resob.png\n");
	}

    } else {
	printf("\tnulla\n");
    }

    cvReleaseMemStorage(&storage);

    return 0;
}
```