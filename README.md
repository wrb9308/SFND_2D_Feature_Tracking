# SFND 2D Feature Tracking

<img src="images/keypoints.png" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, you will now build the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

* First, you will focus on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load. 
* Then, you will integrate several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed. 
* In the next part, you will then focus on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson. 
* In the last part, once the code framework is complete, you will test the various algorithms in different combinations and compare them with regard to some performance measures. 

See the classroom instruction and code comments for more details on each of these parts. Once you are finished with this project, the keypoint matching part will be set up and you can proceed to the next lesson, where the focus is on integrating Lidar points and on object detection using deep-learning. 

## Dependencies for Running Locally
* cmake >= 2.8
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* OpenCV >= 4.1
  * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors.
  * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.

## Writeup
### MP.1 Data buffer optimization
load the images into a vector, when the size of the vector is equal to the predefine size, delete the first image and push back new image.

### MP.2 Keypoint detection 
use string comparision to determine which Detector method was required. Then implement every method in `matching2D_Studen.cpp`.

### MP.3 Keypoint removel
iterate all elements in the keypoints vector, use member function `contains` of class cv::Rect to determine whether keep the keypoint or not.

### MP.4 Keypoint Descriptors
like the ways in MP.2, use string comparision to determine which method to use and implement all descriptor function in `matching2D_Studen.cpp`.

### MP.5 Descriptor Matching
create FLANN matcher per `matcher = cv::makePtr<cv::FlannBasedMatcher>(cv::makePtr<cv::flann::LshIndexParams>(12, 20, 2));`
create `std::vector<std::vector<cv::DMatch>> knn_matches` to store the KNN result for further processing in MP.6

### MP.6 Descriptor Distance Ratio
iterate all elements in knn_matches, use distance ratio filter to compare the distances between two candidate matched keypoint descriptors.

### MP.7 Performance evaluation 1
#### number of keyPoints
|Detector|Image0|Image1|Image2|Image3|Image4|Image5|Image6|Image7|Image8|Image9|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:| :---: |
|HARRIS|17|14|18|21|26|43|18|31|26|34|
|FAST|149|152|150|155|149|149|156|150|138|143|
|BRISK| 264| 282| 282| 277| 297| 279| 289| 272| 266| 254|
|ORB|92| 102| 106| 113| 109| 125| 130| 129| 127| 128| 
|AKAZE|166| 157| 161| 155| 163| 164| 173| 175| 177| 179| 
|SIFT|138| 132| 124| 137| 134| 140| 137| 148| 159| 137| 

#### neighborhood size
|Detector|Image0|Image1|Image2|Image3|Image4|Image5|Image6|Image7|Image8|Image9|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:| :---: |
|HARRIS|17|14|18|21|26|43|18|31|26|34|
|FAST|149|152|150|155|149|149|156|150|138|143|
|BRISK| 264| 282| 282| 277| 297| 279| 289| 272| 266| 254|
|ORB|92| 102| 106| 113| 109| 125| 130| 129| 127| 128| 
|AKAZE|166| 157| 161| 155| 163| 164| 173| 175| 177| 179| 
|SIFT|138| 132| 124| 137| 134| 140| 137| 148| 159| 137| 

### MP.8 Performance evaluation 2 
|Detector/Descriptor|Image0-1|Image1-2|Image2-3|Image3-4|Image4-5|Image5-6|Image6-7|Image7-8|Image8-9|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|SHITOMASI/BRISK|95|88|80|90|82|79|85|86|82|
|SHITOMASI/BRIEF|115|111|104|101|102|102|100|109|100|
|SHITOMASI/ORB|104|103|100|102|103|98|98|102|97|
|SHITOMASI/FREAK|90|88|87|89|83|78|81|86|84|
|SHITOMASI/SIFT|112|109|104|103|99|101|96|106|97|
|SHITOMASI/SIFT|112|109|104|103|99|101|96|106|97|
|HARRIS/BRISK|12|10|14|15|16|16|15|23|21|
|HARRIS/BRIEF|14|11|15|20|24|26|16|24|23|
|HARRIS/ORB|12|13|16|18|24|18|15|24|20|
|HARRIS/FREAK|13|13|15|15|17|20|14|21|18|
|HARRIS/SIFT|14|11|16|19|22|22|13|24|22|
|HARRIS/SIFT|14|11|16|19|22|22|13|24|22|
|FAST/BRISK|97|104|101|98|85|107|107|100|100|
|FAST/BRIEF|119|130|118|126|108|123|131|125|119|
|FAST/ORB|122|122|115|129|107|120|126|122|118|
|FAST/FREAK|97|98|94|99|88|99|104|99|103|
|FAST/SIFT|118|123|110|119|114|119|123|117|103|
|BRISK/BRISK|171|176|157|176|174|188|173|171|184|
|BRISK/BRIEF|178|205|185|179|183|195|207|189|183|
|BRISK/ORB|160|171|157|170|154|180|171|175|172|
|BRISK/FREAK|160|178|156|173|160|183|169|179|168|
|BRISK/SIFT|182|193|169|183|171|195|194|176|183|
|ORB/BRISK|73|74|79|85|79|92|90|88|91|
|ORB/BRIEF|49|43|45|59|53|78|68|84|66|
|ORB/ORB|65|69|71|85|91|101|95|93|91|
|ORB/FREAK|42|36|45|47|44|51|52|49|55|
|ORB/SIFT|67|79|78|79|82|95|95|94|94|
|AKAZE/BRISK|137|125|129|129|131|132|142|146|144|
|AKAZE/BRIEF|141|134|131|130|134|146|150|148|152|
|AKAZE/ORB|130|129|128|115|132|132|137|137|146|
|AKAZE/AKAZE|138|138|133|127|129|146|147|151|150|
|AKAZE/SIFT|134|134|130|136|137|147|147|154|151|
|SIFT/BRISK|64|66|62|66|59|64|64|67|80|
|SIFT/BRIEF|86|78|76|85|69|74|76|70|88|
|SIFT/FREAK|64|72|65|66|63|58|64|65|79|
|SIFT/SIFT|82|81|85|93|90|81|82|102|104|

### MP.9 Performance evaluation 3
#### best combination choice
|Detector/Descriptor|Number of KeyPoints|Runtime|
|:---:|:---:| :---: |
|FAST+BRIEF|122 |1.5 ms|
|FAST+BRISK|99|2.57 ms|
|FAST+ORB|120 |4.1 ms|

#### runtime table
|Detector/Descriptor|Image0|Image1|Image2|Image3|Image4|Image5|Image6|Image7|Image8|Image9|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:| :---: |
|SHITOMASI/BRISK|<ul><li>12.852</li><li>2.00806</li></ul>|<ul><li>16.9392</li><li>2.38758</li></ul>|<ul><li>16.5952</li><li>2.45634</li></ul>|<ul><li>26.6805</li><li>2.46956</li></ul>|<ul><li>17.1997</li><li>1.70522</li></ul>|<ul><li>12.334</li><li>3.05241</li></ul>|<ul><li>17.2867</li><li>1.69937</li></ul>|<ul><li>13.8216</li><li>1.51165</li></ul>|<ul><li>11.595</li><li>1.45073</li></ul>|<ul><li>11.9792</li><li>1.43016</li></ul>|
|SHITOMASI/BRIEF|<ul><li>13.2368</li><li>0.629691</li></ul>|<ul><li>11.6005</li><li>0.879218</li></ul>|<ul><li>11.975</li><li>0.851456</li></ul>|<ul><li>11.6984</li><li>0.855648</li></ul>|<ul><li>11.3568</li><li>0.87815</li></ul>|<ul><li>11.1938</li><li>0.833555</li></ul>|<ul><li>10.8404</li><li>0.823537</li></ul>|<ul><li>11.347</li><li>0.93573</li></ul>|<ul><li>11.4166</li><li>0.82892</li></ul>|<ul><li>16.404</li><li>1.40568</li></ul>|
|SHITOMASI/ORB|<ul><li>16.235</li><li>5.68758</li></ul>|<ul><li>12.6956</li><li>3.72357</li></ul>|<ul><li>11.9559</li><li>3.60474</li></ul>|<ul><li>12.1199</li><li>3.37662</li></ul>|<ul><li>12.0223</li><li>3.48046</li></ul>|<ul><li>11.8006</li><li>3.47284</li></ul>|<ul><li>13.0373</li><li>5.25705</li></ul>|<ul><li>15.3609</li><li>6.1551</li></ul>|<ul><li>17.5567</li><li>6.34603</li></ul>|<ul><li>17.8088</li><li>8.70795</li></ul>|
|SHITOMASI/FREAK|<ul><li>12.3941</li><li>35.7987</li></ul>|<ul><li>11.4585</li><li>35.3349</li></ul>|<ul><li>10.0519</li><li>37.1312</li></ul>|<ul><li>9.79946</li><li>49.4187</li></ul>|<ul><li>13.596</li><li>38.3287</li></ul>|<ul><li>13.7835</li><li>47.2067</li></ul>|<ul><li>13.8032</li><li>50.1589</li></ul>|<ul><li>14.6506</li><li>49.1057</li></ul>|<ul><li>16.7106</li><li>48.7312</li></ul>|<ul><li>13.6006</li><li>68.3108</li></ul>|
|SHITOMASI/SIFT|<ul><li>16.2276</li><li>15.0811</li></ul>|<ul><li>11.5275</li><li>12.837</li></ul>|<ul><li>11.0053</li><li>14.1897</li></ul>|<ul><li>11.7494</li><li>15.9186</li></ul>|<ul><li>11.6162</li><li>17.5317</li></ul>|<ul><li>18.1053</li><li>25.0772</li></ul>|<ul><li>16.7721</li><li>25.1827</li></ul>|<ul><li>17.0947</li><li>25.1093</li></ul>|<ul><li>44.8779</li><li>31.6928</li></ul>|<ul><li>17.572</li><li>23.0789</li></ul>|
|HARRIS/BRISK|<ul><li>13.8422</li><li>0.616368</li></ul>|<ul><li>15.6135</li><li>0.682869</li></ul>|<ul><li>11.5343</li><li>0.849322</li></ul>|<ul><li>17.8559</li><li>1.10351</li></ul>|<ul><li>34.2646</li><li>1.17406</li></ul>|<ul><li>33.1777</li><li>1.39013</li></ul>|<ul><li>16.5501</li><li>0.419178</li></ul>|<ul><li>13.6589</li><li>1.26923</li></ul>|<ul><li>19.4796</li><li>0.997603</li></ul>|<ul><li>21.9394</li><li>0.592223</li></ul>|
|HARRIS/BRIEF|<ul><li>18.9689</li><li>0.411332</li></ul>|<ul><li>17.5248</li><li>0.973571</li></ul>|<ul><li>11.832</li><li>0.610483</li></ul>|<ul><li>11.5051</li><li>0.624458</li></ul>|<ul><li>12.2048</li><li>0.697864</li></ul>|<ul><li>24.6714</li><li>0.390516</li></ul>|<ul><li>15.5803</li><li>0.910581</li></ul>|<ul><li>13.33</li><li>0.727409</li></ul>|<ul><li>15.1448</li><li>0.741248</li></ul>|<ul><li>16.0358</li><li>0.62258</li></ul>|
|HARRIS/ORB|<ul><li>19.5373</li><li>3.47551</li></ul>|<ul><li>13.7217</li><li>3.52811</li></ul>|<ul><li>13.3094</li><li>3.51318</li></ul>|<ul><li>12.5941</li><li>3.57362</li></ul>|<ul><li>12.2138</li><li>3.47208</li></ul>|<ul><li>28.0547</li><li>5.81771</li></ul>|<ul><li>17.84</li><li>6.04211</li></ul>|<ul><li>17.9846</li><li>4.42926</li></ul>|<ul><li>13.2024</li><li>3.49666</li></ul>|<ul><li>18.8913</li><li>4.40872</li></ul>|
|HARRIS/FREAK|<ul><li>14.0781</li><li>35.3759</li></ul>|<ul><li>11.2865</li><li>34.7457</li></ul>|<ul><li>10.1666</li><li>37.7112</li></ul>|<ul><li>15.9143</li><li>49.1278</li></ul>|<ul><li>16.7571</li><li>45.8976</li></ul>|<ul><li>35.6682</li><li>50.8782</li></ul>|<ul><li>15.936</li><li>66.0064</li></ul>|<ul><li>18.0815</li><li>48.4118</li></ul>|<ul><li>16.8317</li><li>48.2093</li></ul>|<ul><li>22.2725</li><li>47.9079</li></ul>|
|HARRIS/SIFT|<ul><li>14.1615</li><li>13.6135</li></ul>|<ul><li>11.6342</li><li>15.2186</li></ul>|<ul><li>11.9619</li><li>14.3663</li></ul>|<ul><li>12.7755</li><li>14.1807</li></ul>|<ul><li>12.0325</li><li>18.8364</li></ul>|<ul><li>23.028</li><li>12.2878</li></ul>|<ul><li>26.6532</li><li>22.0583</li></ul>|<ul><li>20.5406</li><li>20.5458</li></ul>|<ul><li>12.4074</li><li>14.3068</li></ul>|<ul><li>16.3298</li><li>14.6426</li></ul>|
|FAST/BRISK|<ul><li>1.07586</li><li>3.03083</li></ul>|<ul><li>1.06035</li><li>2.86482</li></ul>|<ul><li>0.976783</li><li>2.41611</li></ul>|<ul><li>1.07842</li><li>2.28999</li></ul>|<ul><li>1.10629</li><li>2.21279</li></ul>|<ul><li>0.925972</li><li>1.5681</li></ul>|<ul><li>0.800535</li><li>1.64134</li></ul>|<ul><li>1.1101</li><li>1.61007</li></ul>|<ul><li>1.01734</li><li>1.48045</li></ul>|<ul><li>1.00331</li><li>1.42841</li></ul>|
|FAST/BRIEF|<ul><li>0.872768</li><li>1.08822</li></ul>|<ul><li>0.729399</li><li>1.00947</li></ul>|<ul><li>0.706219</li><li>0.857725</li></ul>|<ul><li>0.908753</li><li>0.668446</li></ul>|<ul><li>0.778165</li><li>0.967986</li></ul>|<ul><li>0.734091</li><li>0.657509</li></ul>|<ul><li>0.728791</li><li>0.649833</li></ul>|<ul><li>0.710073</li><li>0.80428</li></ul>|<ul><li>0.743436</li><li>0.619977</li></ul>|<ul><li>0.718274</li><li>0.916716</li></ul>|
|FAST/ORB|<ul><li>0.717193</li><li>3.77178</li></ul>|<ul><li>0.74819</li><li>3.73582</li></ul>|<ul><li>0.849649</li><li>3.37725</li></ul>|<ul><li>0.775407</li><li>3.43134</li></ul>|<ul><li>0.719706</li><li>3.4328</li></ul>|<ul><li>0.722623</li><li>3.22069</li></ul>|<ul><li>0.732544</li><li>3.24549</li></ul>|<ul><li>0.734841</li><li>3.34747</li></ul>|<ul><li>0.752219</li><li>3.20897</li></ul>|<ul><li>0.736654</li><li>3.28912</li></ul>|
|FAST/FREAK|<ul><li>2.45446</li><li>51.5275</li></ul>|<ul><li>1.1672</li><li>48.9931</li></ul>|<ul><li>1.03451</li><li>46.1926</li></ul>|<ul><li>1.06292</li><li>54.7219</li></ul>|<ul><li>1.05861</li><li>50.7551</li></ul>|<ul><li>1.01976</li><li>49.8489</li></ul>|<ul><li>1.09256</li><li>49.3458</li></ul>|<ul><li>1.04692</li><li>52.2502</li></ul>|<ul><li>1.27133</li><li>49.7344</li></ul>|<ul><li>1.03137</li><li>53.0454</li></ul>|
|FAST/SIFT|<ul><li>1.10694</li><li>25.434</li></ul>|<ul><li>0.746087</li><li>15.6728</li></ul>|<ul><li>0.746921</li><li>15.3899</li></ul>|<ul><li>0.797355</li><li>15.7092</li></ul>|<ul><li>0.793889</li><li>16.1837</li></ul>|<ul><li>0.67867</li><li>16.7866</li></ul>|<ul><li>0.765887</li><li>24.1528</li></ul>|<ul><li>1.06418</li><li>25.1985</li></ul>|<ul><li>1.14703</li><li>21.6271</li></ul>|<ul><li>1.01299</li><li>21.1937</li></ul>|
|BRISK/BRISK|<ul><li>45.9062</li><li>4.49163</li></ul>|<ul><li>54.2481</li><li>4.21572</li></ul>|<ul><li>36.898</li><li>3.97125</li></ul>|<ul><li>36.8611</li><li>2.5152</li></ul>|<ul><li>32.618</li><li>2.78488</li></ul>|<ul><li>31.9342</li><li>2.53211</li></ul>|<ul><li>31.53</li><li>2.72346</li></ul>|<ul><li>31.7902</li><li>2.57169</li></ul>|<ul><li>31.8092</li><li>2.59149</li></ul>|<ul><li>31.9743</li><li>2.4263</li></ul>|
|BRISK/BRIEF|<ul><li>53.6817</li><li>2.22133</li></ul>|<ul><li>56.6465</li><li>1.89226</li></ul>|<ul><li>102.802</li><li>1.76927</li></ul>|<ul><li>70.2837</li><li>2.05311</li></ul>|<ul><li>66.9616</li><li>1.93073</li></ul>|<ul><li>64.3153</li><li>1.82325</li></ul>|<ul><li>44.062</li><li>1.00503</li></ul>|<ul><li>32.9889</li><li>0.97128</li></ul>|<ul><li>33.8659</li><li>0.942472</li></ul>|<ul><li>33.3112</li><li>0.817054</li></ul>|
|BRISK/ORB|<ul><li>57.8529</li><li>21.0461</li></ul>|<ul><li>54.6571</li><li>20.8517</li></ul>|<ul><li>50.8905</li><li>19.7555</li></ul>|<ul><li>56.775</li><li>31.7386</li></ul>|<ul><li>54.9152</li><li>23.4451</li></ul>|<ul><li>54.2312</li><li>21.3833</li></ul>|<ul><li>61.4624</li><li>19.1622</li></ul>|<ul><li>30.969</li><li>11.0708</li></ul>|<ul><li>32.0141</li><li>10.8392</li></ul>|<ul><li>32.5844</li><li>10.8495</li></ul>|
|BRISK/FREAK|<ul><li>38.8675</li><li>46.7739</li></ul>|<ul><li>59.0175</li><li>54.1867</li></ul>|<ul><li>55.5657</li><li>49.8758</li></ul>|<ul><li>55.0543</li><li>39.6603</li></ul>|<ul><li>49.4719</li><li>49.5592</li></ul>|<ul><li>34.5624</li><li>38.862</li></ul>|<ul><li>32.1663</li><li>36.0138</li></ul>|<ul><li>31.5612</li><li>36.3596</li></ul>|<ul><li>31.3899</li><li>36.0461</li></ul>|<ul><li>31.9233</li><li>36.0272</li></ul>|
|BRISK/SIFT|<ul><li>54.2755</li><li>96.5254</li></ul>|<ul><li>57.4064</li><li>60.313</li></ul>|<ul><li>56.504</li><li>82.843</li></ul>|<ul><li>52.3507</li><li>60.7561</li></ul>|<ul><li>49.1201</li><li>46.6392</li></ul>|<ul><li>45.0496</li><li>37.3122</li></ul>|<ul><li>31.8997</li><li>27.016</li></ul>|<ul><li>31.7543</li><li>26.6202</li></ul>|<ul><li>32.3412</li><li>28.8086</li></ul>|<ul><li>32.1734</li><li>25.1581</li></ul>|
|ORB/BRISK|<ul><li>72.6377</li><li>2.16437</li></ul>|<ul><li>8.73331</li><li>1.80621</li></ul>|<ul><li>12.3164</li><li>1.85131</li></ul>|<ul><li>13.057</li><li>1.93161</li></ul>|<ul><li>13.3315</li><li>1.91424</li></ul>|<ul><li>11.9402</li><li>2.18317</li></ul>|<ul><li>12.3611</li><li>2.37216</li></ul>|<ul><li>15.39</li><li>1.39316</li></ul>|<ul><li>6.69201</li><li>1.29943</li></ul>|<ul><li>6.45607</li><li>1.42107</li></ul>|
|ORB/BRIEF|<ul><li>89.9844</li><li>0.815043</li></ul>|<ul><li>9.04421</li><li>0.839141</li></ul>|<ul><li>9.70432</li><li>0.53622</li></ul>|<ul><li>9.04322</li><li>0.815479</li></ul>|<ul><li>8.02861</li><li>0.564877</li></ul>|<ul><li>8.08331</li><li>0.607394</li></ul>|<ul><li>7.48391</li><li>0.570182</li></ul>|<ul><li>6.83001</li><li>0.556034</li></ul>|<ul><li>11.5911</li><li>1.02549</li></ul>|<ul><li>8.21315</li><li>0.584717</li></ul>|
|ORB/ORB|<ul><li>76.6721</li><li>12.6729</li></ul>|<ul><li>6.55281</li><li>12.1352</li></ul>|<ul><li>6.2855</li><li>21.9145</li></ul>|<ul><li>11.3125</li><li>21.2594</li></ul>|<ul><li>9.26776</li><li>18.8417</li></ul>|<ul><li>7.8476</li><li>23.8104</li></ul>|<ul><li>10.3695</li><li>22.8664</li></ul>|<ul><li>7.10972</li><li>26.9086</li></ul>|<ul><li>12.1242</li><li>24.2714</li></ul>|<ul><li>13.5036</li><li>24.3239</li></ul>|
|ORB/FREAK|<ul><li>76.488</li><li>35.3894</li></ul>|<ul><li>6.18107</li><li>40.4431</li></ul>|<ul><li>6.47385</li><li>46.6782</li></ul>|<ul><li>13.2672</li><li>49.945</li></ul>|<ul><li>13.8794</li><li>49.5372</li></ul>|<ul><li>13.0961</li><li>81.3946</li></ul>|<ul><li>14.0635</li><li>50.5612</li></ul>|<ul><li>13.1722</li><li>49.5238</li></ul>|<ul><li>13.3155</li><li>49.2819</li></ul>|<ul><li>9.67225</li><li>58.5064</li></ul>|
|ORB/SIFT|<ul><li>77.5847</li><li>36.2168</li></ul>|<ul><li>6.32999</li><li>38.8018</li></ul>|<ul><li>11.4368</li><li>52.3489</li></ul>|<ul><li>10.3136</li><li>50.7703</li></ul>|<ul><li>10.5954</li><li>44.7001</li></ul>|<ul><li>9.445</li><li>51.2275</li></ul>|<ul><li>12.896</li><li>54.0913</li></ul>|<ul><li>10.5274</li><li>78.3425</li></ul>|<ul><li>12.3314</li><li>99.21</li></ul>|<ul><li>14.2721</li><li>61.9492</li></ul>|
|AKAZE/BRISK|<ul><li>61.1062</li><li>2.54075</li></ul>|<ul><li>148.577</li><li>2.34393</li></ul>|<ul><li>143.171</li><li>3.00691</li></ul>|<ul><li>149.599</li><li>2.325</li></ul>|<ul><li>88.2218</li><li>2.9427</li></ul>|<ul><li>100.227</li><li>1.56222</li></ul>|<ul><li>60.983</li><li>1.62842</li></ul>|<ul><li>57.2656</li><li>1.65766</li></ul>|<ul><li>59.1304</li><li>1.79933</li></ul>|<ul><li>57.2514</li><li>2.01885</li></ul>|
|AKAZE/BRIEF|<ul><li>66.5943</li><li>0.832584</li></ul>|<ul><li>57.8521</li><li>0.690219</li></ul>|<ul><li>87.1009</li><li>0.782277</li></ul>|<ul><li>96.0351</li><li>1.1009</li></ul>|<ul><li>117.782</li><li>1.24241</li></ul>|<ul><li>138.635</li><li>1.99257</li></ul>|<ul><li>148.119</li><li>1.26663</li></ul>|<ul><li>107.165</li><li>1.24012</li></ul>|<ul><li>174.37</li><li>1.29016</li></ul>|<ul><li>141.775</li><li>1.81245</li></ul>|
|AKAZE/ORB|<ul><li>61.1554</li><li>8.59729</li></ul>|<ul><li>59.2968</li><li>8.59033</li></ul>|<ul><li>87.0625</li><li>9.67052</li></ul>|<ul><li>86.2675</li><li>10.3273</li></ul>|<ul><li>86.6347</li><li>10.1719</li></ul>|<ul><li>106.929</li><li>15.2898</li></ul>|<ul><li>136.261</li><li>15.7449</li></ul>|<ul><li>148.915</li><li>26.9879</li></ul>|<ul><li>110.188</li><li>14.2117</li></ul>|<ul><li>119.126</li><li>16.4928</li></ul>|
|AKAZE/FREAK|<ul><li>69.7968</li><li>38.3637</li></ul>|<ul><li>71.1634</li><li>40.9123</li></ul>|<ul><li>75.7114</li><li>37.4777</li></ul>|<ul><li>107.846</li><li>51.2255</li></ul>|<ul><li>124.007</li><li>50.6805</li></ul>|<ul><li>167.783</li><li>49.8154</li></ul>|<ul><li>107.559</li><li>64.71</li></ul>|<ul><li>126.852</li><li>50.4547</li></ul>|<ul><li>162.55</li><li>48.7639</li></ul>|<ul><li>103.997</li><li>52.3768</li></ul>|
|AKAZE/AKAZE|<ul><li>102.373</li><li>65.5178</li></ul>|<ul><li>95.8423</li><li>66.0853</li></ul>|<ul><li>72.2145</li><li>86.0501</li></ul>|<ul><li>113.824</li><li>101.049</li></ul>|<ul><li>120.204</li><li>86.1985</li></ul>|<ul><li>123.141</li><li>119.089</li></ul>|<ul><li>113.466</li><li>127.713</li></ul>|<ul><li>97.2626</li><li>113.012</li></ul>|<ul><li>72.2684</li><li>69.1608</li></ul>|<ul><li>113.776</li><li>116.523</li></ul>|
|AKAZE/SIFT|<ul><li>66.8616</li><li>20.0028</li></ul>|<ul><li>63.1743</li><li>29.7388</li></ul>|<ul><li>87.2595</li><li>23.7627</li></ul>|<ul><li>90.9488</li><li>42.1693</li></ul>|<ul><li>129.467</li><li>25.2081</li></ul>|<ul><li>132.495</li><li>35.13</li></ul>|<ul><li>174.194</li><li>34.3831</li></ul>|<ul><li>134.478</li><li>33.672</li></ul>|<ul><li>125.083</li><li>27.4726</li></ul>|<ul><li>81.533</li><li>26.3194</li></ul>|
|SIFT/BRISK|<ul><li>111.348</li><li>2.39914</li></ul>|<ul><li>193.507</li><li>2.05624</li></ul>|<ul><li>174.179</li><li>1.81314</li></ul>|<ul><li>179.65</li><li>1.99686</li></ul>|<ul><li>167.357</li><li>2.03804</li></ul>|<ul><li>144.708</li><li>1.37216</li></ul>|<ul><li>84.2419</li><li>1.36559</li></ul>|<ul><li>92.3414</li><li>1.46256</li></ul>|<ul><li>87.4296</li><li>1.65583</li></ul>|<ul><li>92.5408</li><li>1.49777</li></ul>|
|SIFT/BRIEF|<ul><li>94.9739</li><li>0.657677</li></ul>|<ul><li>113.213</li><li>0.628668</li></ul>|<ul><li>114.497</li><li>0.622925</li></ul>|<ul><li>185.109</li><li>2.52357</li></ul>|<ul><li>202.32</li><li>1.62454</li></ul>|<ul><li>168.533</li><li>1.13254</li></ul>|<ul><li>226.149</li><li>1.20977</li></ul>|<ul><li>175.444</li><li>1.1589</li></ul>|<ul><li>166.292</li><li>1.21973</li></ul>|<ul><li>142.664</li><li>1.42343</li></ul>|
|SIFT/FREAK|<ul><li>101.488</li><li>37.7531</li></ul>|<ul><li>113.843</li><li>48.5027</li></ul>|<ul><li>100.616</li><li>37.989</li></ul>|<ul><li>99.063</li><li>38.0312</li></ul>|<ul><li>101.475</li><li>37.3615</li></ul>|<ul><li>95.6901</li><li>37.4292</li></ul>|<ul><li>95.2923</li><li>37.7597</li></ul>|<ul><li>94.211</li><li>37.2219</li></ul>|<ul><li>93.5529</li><li>37.6143</li></ul>|<ul><li>96.4288</li><li>37.681</li></ul>|
|SIFT/SIFT|<ul><li>105.077</li><li>80.7363</li></ul>|<ul><li>144.296</li><li>77.7669</li></ul>|<ul><li>174.804</li><li>132.222</li></ul>|<ul><li>306.938</li><li>127.475</li></ul>|<ul><li>115.408</li><li>98.8066</li></ul>|<ul><li>188.833</li><li>100.832</li></ul>|<ul><li>209.902</li><li>114.794</li></ul>|<ul><li>203.043</li><li>94.5684</li></ul>|<ul><li>154.21</li><li>90.1211</li></ul>|<ul><li>168.436</li><li>91.9096</li></ul>|

