## Train Samples

You will need some executables like `opencv_createsamples`, `opencv_haartraining` and `convert_cascade`. To install those you can follow the docs under [OpenCV Docs](./OpenCV.md). For this example create three folders and fill them with the necessary data:

* `positive_images`: Save all cropped images with content that you want to track
* `negative_images`: Save other images, giving a lot of examples, og images from same genre but should not contain any part of postive images (cropped parts)
* `samples`: Leave it empty for now


### Creating Train Data Files

In the folder named "positive_images", the images are in png format and in "negative_images", they are in jpeg format. We will run the commends above to create `positives.dat` and `negatives.dat` containing all the images paths unified in one `dat` file.

```sh
find ./negative_images -name '*.jpeg' > negatives.dat
find ./positive_images -name '*.png' > positives.dat
```

## Create samples

Based on the positive and negative numbers, the `opencv_createsamples` tool will create a lot of images based on the samplesyou have provided, applying distortions and other variations to increase our sample data. 

The arguments for this command are:
* `positives.dat`: List of positive image paths
* `negatives.dat`: List of negative image paths
* `samples`: Folder that stores the data of training samples
* `250`: Number of training samples
* `-w 160 -h 20`: Width and height ratio of the object you are looking for. For example, a face has `-w 20 -h 20`, a pen will be `160 and 20`

```sh
perl createtrainsamples.pl positives.dat negatives.dat samples 250 "./opencv_createsamples  -bgcolor 0 -bgthresh 0 -maxxangle 1.1 -maxyangle 1.1 maxzangle 0.5 -maxidev 40 -w 160 -h 20"
```


## Final Train Data

Now that we have built or sample folder with data, we can unify this into one training data to be used on the HAAR Training

```sh
find samples/ -name '*.vec' > samples.dat
./mergevec samples.dat samples.vec
```

## Creates the HaarCascade

This creates the haarcascade xml file desired by us in the same Haar_training folder, keeping all the meta data, that is forms in the meanwhile in the haarcascade folder, which is created automatically. The arguements to be noted in the above command are

* `haarcascade`: The folder in which meta data can be kept
* `samples.vec`: The unified training data, that we created just a while ago, before this command
* `negatives.dat`: The list of negative image paths
* `20`: Stages, the more, the better, but great time consumer
* `99`: No. of negative images
* `2048`: The RAM memory that can be used

```sh
opencv_haartraining -data haarcascade -vec samples.vec -bg negatives.dat -nstages 20 -nsplits 2 -minhitrate 0.999 -maxfalsealarm 0.5 -npos 250 -nneg 99 -w 160 -h 20 -nonsym -mem 2048 -mode ALL
```

### Debug

This process can take hours to complete and generate the final object detector. You can keep traking the progress of this process generating temporary detector.
So at any time you can run the following command to generate the XML file and test the generator as it goes

```sh
./convert_cascade --size="160x20" haarcascade haarcascade-inter.xml
```



## References:

* [Mobile Detection](https://memememememememe.me/post/training-haar-cascades/)
* [Pen Detector](http://opencvuser.blogspot.com/2011/08/creating-haar-cascade-classifier-aka.html)
* [Tutorial 1](http://note.sonots.com/SciSoftware/haartraining.html)
* [Tutorial 2](https://towardsdatascience.com/computer-vision-detecting-objects-using-haar-cascade-classifier-4585472829a9)
* [Tutorial 3](https://pythonprogramming.net/haar-cascade-object-detection-python-opencv-tutorial/)

### GUI

 If you are under Windows,you might try the 
 * [Haar GUI](http://www.jackyle.com/2017/11/haar-cascade-training-on-windows-by-gui.html)
 * [Haar GUI Tutorial](https://medium.com/@vipulgote4/guide-to-make-custom-haar-cascade-xml-file-for-object-detection-with-opencv-6932e22c3f0e)