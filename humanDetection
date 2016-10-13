#include <iostream>
#include <opencv2/opencv.hpp>

using namespace std;
using namespace cv;

int main(int argc, const char * argv[])
{
	//VideoCapture cap(CV_CAP_ANY);
	VideoCapture cap("C:\\RecordDownload\\v6.avi");
	
	if (!cap.isOpened())
		return -1;
	
	Mat img;
	HOGDescriptor hog;
	hog.setSVMDetector(HOGDescriptor::getDefaultPeopleDetector());

	namedWindow("video capture", CV_WINDOW_AUTOSIZE);

	// Escrbiendo el video de salida
	Mat src;
	cap >> src;
	bool isColor = (src.type() == CV_8UC3);
	VideoWriter writer;
	
	int codec = CV_FOURCC('M', 'J', 'P', 'G');  // select desired codec (must be available at runtime)
	double fps = 25.0;                          // framerate of the created video stream
	string filename = "./live3.avi";             // name of the output video file
	writer.open(filename, codec, fps, cv::Size(850, 500), isColor);

	// check if we succeeded
	if (!writer.isOpened()) {
		cerr << "Could not open the output video file for write\n";
		return -1;
	}

	// 

	while (true)
	{
		
		cap >> img;
		if (!img.data)
			continue;

		resize(img, img, cv::Size(850, 500));

		vector<Rect> found, found_filtered;
		hog.detectMultiScale(img, found, 0, Size(8, 8), Size(32, 32), 1.05, 2);

		size_t i, j;
		for (i = 0; i<found.size(); i++)
		{
			Rect r = found[i];
			for (j = 0; j<found.size(); j++)
				if (j != i && (r & found[j]) == r)
					break;
			if (j == found.size())
				found_filtered.push_back(r);
		}
		for (i = 0; i<found_filtered.size(); i++)
		{
			Rect r = found_filtered[i];
			r.x += cvRound(r.width*0.1);
			r.width = cvRound(r.width*0.8);
			r.y += cvRound(r.height*0.06);
			r.height = cvRound(r.height*0.9);
			rectangle(img, r.tl(), r.br(), cv::Scalar(0, 255, 0), 2);
		}
		imshow("video capture", img);
		if (waitKey(20) >= 0)
			break;

		writer.write(img);
	}
	return 0;
}
