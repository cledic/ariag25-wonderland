
```
// Capture.c
//
// Example showing how to connect to a webcam and capture
// video frames

#include "stdio.h"
#include "string.h"
//#include "cv.h"
#include "highgui.h"

int main(int argc, char ** argv)
{
  CvCapture * pCapture    = 0;
  IplImage *  pVideoFrame = 0;
  int         i;
  char        filename[50];

  // Initialize video capture
  pCapture = cvCaptureFromCAM( CV_CAP_ANY );
  if( !pCapture )
  {
    fprintf(stderr, "failed to initialize video capture\n");
    return -1;
  }

  // Capture three video frames and write them as files
  for(i=0; i<3; i++)
  {
    pVideoFrame = cvQueryFrame( pCapture );
    if( !pVideoFrame )
    {
      fprintf(stderr, "failed to get a video frame\n");
    }

    // Write the captured video frame as an image file
    sprintf(filename, "VideoFrame%d.png", i+1);
    if( !cvSaveImage( filename, pVideoFrame) )
    {
      fprintf(stderr, "failed to write image file %s\n", filename);
    }

    // IMPORTANT: Don't release or modify the image returned
    // from cvQueryFrame() !
  }

  // Terminate video capture and free capture resources
  cvReleaseCapture( &pCapture );

  return 0;
}
```