# Image Retrieval using Color Difference Histogram
Implementation of Content Based Image Retrieval Process based on Color Difference Histogram described by Guang-Hai Liu et al. in the [paper](https://doi.org/10.1016/j.patcog.2012.06.001) using Python. The project was done under the guidance of [Prof. Kumar](https://research.vit.ac.in/researcher/naveen-kumar-n)

## Color Difference Histogram
Color Difference Histogram (CDH) method counts the perceptually uniform color difference between two points under different backgrounds with regard to colors and edge orientations in L\*a\*b\* colorspace because the visual perceptual differences between two colors in L\*a\*b\* colorspace are related to a measure of Euclidean distance while R, G and B components are highly correlated, and therefore, chromatic information is not directly fit for use. CDH also takes into account the spatial layout without any image segmentation, learning processes or any clustering implementation.

#### Algorithm
The steps involved in the CDH are:
- Convert the image to L\*a\*b\* colorspace image
- Edge orientation detection in L\*a\*b\* colorspace
because if the gradient magnitude and orientation are detected based on the gray-scale image, much chromatic information will be lost.
- Use the Sobel operator for detection (because it is less sensitive to noise and has a small computational burden).
- Color quantization in the L\*a\*b\* colorspace
we quantize the L\* channel into 10 bins and the a\* and b\* channels into 3 bins; therefore, 10\*3\*3 = 90 color combinations are obtained.
- Feature Extraction: only edge orientations and color index values that are the same are selected to calculate the color difference histogram,rather than all of them.

Canberra distance is taken as the distance measure over Euclidean or Manhattan distance, because the distances in each dimensions are squared before summation, placing great emphasis on features that are greatly dissimilar.

Color Difference Histogram algorithm can be considered as an improved Multi-Texton Histogram (MTH) because it considers the same neighboring colors and edge orientations as texton types and is not just limited to four special texton types.

#### Repository Structure
- Color Difference Histogram-A.ipynb: Jupyter Notebook used for explaining code for extracting features using color difference histogram of an input image
- CBIR Using Color Difference Histogram - Retrieval.ipynb: Jupyter Notebook used for explaining code for retrieval of image from the MongoDB database.
- CDH.py: Python code responsible for extracting the features from the images and seeding it into MongoDB database.
- dump/CDHCorel: Folder containing the actual dump (108 bin feature-vector for each image) of the seeded images in the database. It can be restored as:
```
cd dump/CDHCorel/
mongorestore --db db_name .
```
-  Corel10k : The images dataset downloaded from the link given below. Download the dataset and put all the images in the directory named `Corel10k` which is made in the same folder as the notebook that you are executing.

### Dataset used
Dataset used for the project is Corel-10k dataset which contains 100 categories, and there are 10,000 images from diverse contents such as sunset, beach, flower, building, car, horses, mountains, fish, food, door, etc. Each category contains 100 images of size 192×128 or 128×192 in the JPEG format.  The dataset can be downloaded from [Corel-10K](http://www.ci.gxnu.edu.cn/cbir/Dataset.aspx)

# Working
### Sample Input: 
![input](https://i.imgur.com/J533tjQ.jpg)

The sample input is taken as per the algorithm described above, the processing is taken place.
After which the top results which are similar to the query image are returned.

### Sample Output:
![output1](https://i.imgur.com/p5X22zO.jpg)
![output2](https://i.imgur.com/Emrdnx3.jpg)
![output3](https://i.imgur.com/VOBowyi.jpg)
![output4](https://i.imgur.com/P5Ugctf.jpg)
![output5](https://i.imgur.com/eZICJUO.jpg)
![output6](https://i.imgur.com/YTalMKE.jpg)

The outputs are more or less similar except for one (the helicopter photo)which is an outlier 
but still it is having an accuracy of around about 83% for top 6 images.
