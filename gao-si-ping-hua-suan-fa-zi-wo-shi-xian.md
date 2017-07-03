```
int testGaussion(void){

    Mat srcImage;
    Mat grayImage;

    int mWidth;
    int mHeight;

    srcImage = imread("D:\lena.jpg");

    if(srcImage.empty() == true){

        cout<<"get source Image failed"<<endl;
        return  -1;
    }

    //show source Image
//    cvNamedWindow("src",CV_WINDOW_AUTOSIZE);

 //   imshow("src",srcImage);

    //make source Image to gray Image

    cvtColor(srcImage,grayImage,CV_BGR2GRAY);

    cvNamedWindow("gray",CV_WINDOW_AUTOSIZE);

    imshow("gray",grayImage);

    //get width / height of grayImage

    mWidth = grayImage.cols;
    mHeight = grayImage.rows;

    cout<<"mWidth is "<<mWidth<<endl;
    cout<<"mHeight is "<<mHeight<<endl;

    uchar** arrayImage = new uchar*[grayImage.rows];
    uchar** dstImage = new uchar*[grayImage.rows];

    IplImage* testImage = cvCreateImage(CvSize(grayImage.rows,grayImage.cols),IPL_DEPTH_8U,1);

    if(grayImage.isContinuous() == true){

        for(int i = 0;i < grayImage.rows;i++){
              arrayImage[i] = new uchar[grayImage.cols];
              arrayImage[i] = grayImage.ptr<uchar>(i);
              dstImage[i] = new uchar[grayImage.cols];
        }

        for(int i = 1;i < grayImage.rows-1;i++){
            for(int j = 1;j<grayImage.cols-1;j++){
                int tmpSum = arrayImage[i-1][j-1]+arrayImage[i-1][j+1]+arrayImage[i+1][j-1]+arrayImage[i+1][j+1];
                tmpSum += 2*(arrayImage[i-1][j]+arrayImage[i][j-1]+arrayImage[i][j+1]+arrayImage[i+1][j]);
                tmpSum += 4*arrayImage[i][j];
                tmpSum /= 16;
                cout<<"tmpSum:"<<tmpSum<<endl;
                if(tmpSum > 255){

                    tmpSum = 255;
                }

                dstImage[i][j]=tmpSum;
            }
        }
        for(int i = 0;i<grayImage.rows;i++){
            for(int j = 0;j <grayImage.cols;j++)
                testImage->imageData[i*grayImage.cols+j]=dstImage[i][j];
        }

    }else{
        cout<<"the Image is not continuoue, translate to array failed"<<endl;

        return -1;
    }
    cvNamedWindow("test",CV_WINDOW_AUTOSIZE);
    cvShowImage("test",testImage);
    cvWaitKey(0);
    return 0;
}
```



