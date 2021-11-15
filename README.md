# Anomaly-Detection-Using-IMU-Data


In here we use given [IMU](https://www.arrow.com/en/research-and-events/articles/imu-principles-and-applications)(Inertial Measurement Unit) data of a vehicle to detect anomalies. 
The data was in rodbag file. [ROS](https://www.ros.org/)(Robot Operating System)

### Initial Requirements
> Python

> Bagpy

> Pandas

> Numpy

> Matplotlib

> Sklearn

> Tensorflow

> Keras



### Steps

* you have to extract the data from given rosbag file. There are so many data in it. Then filterout the IMU data. You can use [this method](https://github.com/Chumsy0725/ICAS-2021/blob/main/IMU/extract-data/extract_data.ipynb) fot that. 
* Save the extracted data in csv file for further processing techniques. You can see something like this for IMU data



| Time         | header.seq  | header.stamp.secs | header.frame_id | orientation.x | ... | orientation.y | linear_acceleration_covariance_7 | linear_acceleration_covariance_8 |
| -----------  | ----------- | ----------------- | --------------- | ------------- | --- | ------------- | -------------------------------- | -------------------------------- |
| 1.6167e+09   | 123648      | 1616766027	       | fcu             | -0.036490     | ... | 0.021663      | 0.0                              | 9.000000e-08                     |
| 1.6167e+09   | 123649      | 1616766027        | fcu             | -0.036553     | ... | 0.021804      | 0.0                              | 9.000000e-08                     |
| 1.6167e+09   | 123650      | 1616766027        | fcu             | -0.036523     | ... | 0.022071      | 0.0                              | 9.000000e-08                     |
| 1.6167e+09   | 123651      | 1616766027        | fcu             | -0.036695     | ... | 0.022406      | 0.0                              | 9.000000e-08                     |
| ............ | ........... | ................. | ............... | ............. | ... | ............. | ................................ | ................................ |
| ............ | ........... | ................. | ............... | ............. | ... | ............. | ................................ | ................................ |
| ............ | ........... | ................. | ............... | ............. | ... | ............. | ................................ | ................................ |




* After that you have to crate a [deep learning model for forcasting](https://github.com/Chumsy0725/ICAS-2021/blob/main/IMU/Model%20Training/Multivariate_LSTM_Model.ipynb)...
*You can simply follow the code for more understanding*

  * For forecasting I only used 6 IMU data features
  * > `angular_velocity.x`  `angular_velocity.y` `angular_velocity.z` `linear_acceleration.x` `linear_acceleration.y` `linear_acceleration.z`
  * Preprocess the data set and reshape the data set. No of steps = 3
      * 6 features are split to 3 consecutive data samples. Take it as the Input(X) and consecutive 4th Data as output(Y)
  * Split as Train & Test data, Scale to between 0 and 1
  * Build a LSTM Forcaster model and Check for the accuracies after fitting the model
  * Results...
      * `Training Loss ~ 0.15%`  `Validation Loss ~ 1.18%`  `Training Accuracy ~ 81.59%`  `Validation Accuracy ~ 44.83%` 


<img src="https://github.com/Chumsy0725/ICAS-2021/blob/main/IMU/Model%20Training/validation_accuracy.png" alt="Accuracies and Losses" style="width:1000px;"/>


  * Capturing the Abnormalities...


  <img src="https://github.com/Chumsy0725/ICAS-2021/blob/main/IMU/Model%20Training/abnormal_error.png" alt="Checking Abnormalities" style="width:1000px;"/>
