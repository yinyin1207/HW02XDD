#include "sprout_opencv.h"
#include <iostream>
#include <vector>

int main(void)
{
	SproutMatrix imageA = loadImage("./images/faces.jpg");
	SproutMatrix mask = createNewImageWithRGB (349, 620, 0, 0, 0);
	SproutMatrix imageB = copyImage(imageA);
	SproutMatrix nothing = gaussianBlurOnImage(imageB, 21*21, 5);
	/*displayImageWithTitle(nothing, "pu~~~~~");
	waitKeyInput();*/
	std::vector<SproutRectangle> faceplace;
	faceplace = getFacesFromMatrixWithSize(imageA, 90, 117);
	for(int i = 0; i<faceplace.size(); i++)
	{
		SproutPoint point = SproutPoint(faceplace[i].x + sproutRound(faceplace[i].width / 2),
										faceplace[i].y + sproutRound(faceplace[i].height / 2));
		int radius = sproutRound(faceplace[i].height / 2);
		drawCircleOnImage (mask, point, radius, 255, 255, 255);
	}
	//displayImageWithTitle(mask, "Black~");
	//waitKeyInput();
	SproutMatrix kaonashi = copyImageWithMask(imageA, nothing, mask);
	/*displayImageWithTitle(kaonashi, "XD");
	waitKeyInput();*/
	writeImage("./images/facesblur.jpg", kaonashi);
	return 0;
}
