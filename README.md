# Steps for running CNN-DSO data (adapted from original README)

## 1. Installation
### Dependencies
### DSO
- Setup dependencies of DSO (https://github.com/JakobEngel/dso)
#### 1.1 Required Dependencies
The following instructions are tested on Ubuntu 16.04 and 18.04. Other platforms might work with minor adjustments.
##### eigen3 and boost (required).
Required. Install with

    sudo apt-get install libeigen3-dev libboost-all-dev

#### 1.2 Optional Dependencies

##### OpenCV (highly recommended).
Used to read / write / display images.
OpenCV is **only** used in `IOWrapper/OpenCV/*`. Without OpenCV, respective 
dummy functions from `IOWrapper/*_dummy.cpp` will be compiled into the library, which do nothing.
The main binary will not be created, since it is useless if it can't read the datasets from disk.
Feel free to implement your own version of these functions with your prefered library, 
if you want to stay away from OpenCV.

Install with

	sudo apt-get install libopencv-dev


##### Pangolin (highly recommended).
Used for 3D visualization & the GUI.
Pangolin is **only** used in `IOWrapper/Pangolin/*`. You can compile without Pangolin, 
however then there is not going to be any visualization / GUI capability. 

Install from [https://github.com/stevenlovegrove/Pangolin](https://github.com/stevenlovegrove/Pangolin)

### Monodepth
- Build TensorFlow C++ API (https://github.com/yx0123/monodepth-cpp/tree/master/Tensorflow_build_instructions). **This is the hardest part!** 
- Build monodepth-cpp (https://github.com/yx0123/monodepth-cpp). 
- Prepare Monodepth pre-trained model. You can freaze .ckpt or download the model trained on cityscapes and fine-tuned on kitti [here](https://github.com/yan99033/monodepth-cpp/tree/master/model). 



# Original README below:

# CNN-DSO: A combination of Direct Sparse Odometry and CNN Depth Prediction 

### 1. Overview
<p align="center"> <img src="https://github.com/muskie82/CNN-DSO/blob/master/gif/demo.gif" width="500" height="320"> </p>

This code provides a combination of [DSO](https://vision.in.tum.de/research/vslam/dso) and [Monodepth](http://visual.cs.ucl.ac.uk/pubs/monoDepth/).
For every keyframe, depth values are initialized with the prediction from Monodepth.

Absolute keyframe trajectory RMSE (in meter)
on KITTI dataset (DSO and ORB-SLAM numbers are from [CNN-SVO paper](https://arxiv.org/pdf/1810.01011.pdf))

|Sequence on KITTI|CNN-DSO|DSO|ORB-SLAM|
|---|---|---|---:|
|00|**15.13**|113.18|77.95|
|01|**5.901**|X|X|
|02|**12.53**|116.81|41.00|
|03|1.516|1.3943|**1.018**|
|04|**0.100**|0.422|0.930|
|05|**20.3**|47.46|40.35|
|06|**1.547**|55.61|52.22|
|07|**8.369**|16.71|16.54|
|08|**10.53**|111.08|51.62|
|09|**14.00**|52.22|58.17|
|10|**4.10**|11.09|18.47|

### 2. Installation
#### 2.1 Dependencies
##### DSO
- Setup dependencies of DSO (https://github.com/JakobEngel/dso)

##### Monodepth
- Build TensorFlow C++ API (https://github.com/yan99033/monodepth-cpp/tree/master/Tensorflow_build_instructions). **This is the hardest part!** 
- Build monodepth-cpp (https://github.com/yan99033/monodepth-cpp). 
- Prepare Monodepth pre-trained model. You can freaze .ckpt or download the model trained on cityscapes and fine-tuned on kitti [here](https://github.com/yan99033/monodepth-cpp/tree/master/model). 


#### 2.3 Build

- Download the repository.

		git clone https://github.com/muskie82/CNN-DSO.git

- Modify paths to include directories and libraries of TensorFlow and monodepth-cpp in `CMakeLists.txt` (4 lines of `/abosolute/path/to/XXXXX`).

- Build

		cd CNN-DSO
		mkdir build
		cd build
		cmake ..
		make -j4
	

### 3 Usage
In addition to original DSO command line, you should specify the path to pre-trained model by `cnn`.

		bin/dso_dataset \
			files=XXXXX/sequence_XX/image_0 \
			calib=XXXXX/sequence_XX/camera.txt \
			cnn=XXXXX/model_city2kitti.pb \
			preset=0 \
			mode=1


### 4 Reference
* **Direct Sparse Odometry**, *Engel, Jakob, Vladlen Koltun, and Daniel Cremers*, IEEE transactions on pattern analysis and machine intelligence 40.3 (2018): 611-625. (https://github.com/JakobEngel/dso)
* **Unsupervised monocular depth estimation with left-right consistency**, *Godard, Clément, Oisin Mac Aodha, and Gabriel J. Brostow. *, CVPR. Vol. 2. No. 6. 2017. (https://github.com/mrharicot/monodepth)
* **CNN-SVO: Improving the Mapping in Semi-Direct Visual Odometry Using Single-Image Depth Prediction.**, Loo, S. Y., Amiri, A. J., Mashohor, S., Tang, S. H., and Zhang, H, arXiv preprint arXiv:1810.01011 (2018).
(https://github.com/yan99033/CNN-SVO)
* **stereo_dso by HorizonAD:** https://github.com/HorizonAD/stereo_dso

### 5 License
GPLv3 license.
I don't take any credit from DSO, Monodepth and monodepth-cpp. Please check them.
