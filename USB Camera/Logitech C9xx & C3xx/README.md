## Download & Install Basic prerequisites :
You need to have ROS installed with a 'catkin_ws' directory created already. Clone the 'usb_cam' package into the 'src' directory inside 'catkin_ws':
```
cd ~/catkin_ws/src/
git clone https://github.com/bosch-ros-pkg/usb_cam.git
```
Build the workspace using catkin_make:
```
cd ..
catkin_make
```
After building the package, install the v4l-util Ubuntu package. It is a collection of command-line V4L (Video for Linux) utilities used by the usb_cam package:
```
sudo apt-get install v4l-utils
```
Now, copy the camera_info directory from ~/catkin_ws/src/usb-cam & paste to ~/.ros directory at your 'Home' directory. You can see by default a 'head_camera.yaml' file (camera calibration file) inside camera_info directory for Logitech C930e opening at 640x480 resolution. This is needed when using usb_cam package. In actual case, you have to manually calibrate your camera to generate this file (which is brand & resolution specific). 
## Note: 
The camera brands used here are Logitech C310 & C930e. 'Calibration file' is camera brand as well as resolution specific. The calibration files for 2 camera brands & corresponding resolution has been included inside 'usb_cam' package of this repo. You can just rename one of the files to 'head_camera.yaml' & paste it inside ~/.ros/camera_info directory.  
## Configuring USB-Webcam on Ubuntu 18.04:
After installing these two, we can connect the webcam to the PC to check whether it is properly detected by our PC. Open a Terminal and execute the dmesg command to check the kernel logs. 
```
dmesg
```
If your camera is detected in Linux, it may give you logs like this: 
```
[ 1298.805115] usb 1-1: new high-speed USB device number 6 using xhci_hcd
[ 1298.805115] usb 1-1: New USB device found, idVendor=046d, idProduct=0843, bcdDevice= 0.13
[ 1298.805120] usb 1-1: New USB device strings: Mfr=0, Product=2, SerialNumber=1
[ 1298.805123] usb 1-1: Product: Logitech Webcam C930e
[ 1298.805127] usb 1-1: SerialNumber: E0E1D17E
[ 1298.806245] uvcvideo: Found UVC 1.00 device Logitech Webcam C930e (046d:0843)
```
You can use any webcam that has driver support in Linux. We can also check which usb devices are attached with our PC or laptop with the following command:
```
ls /dev/video*
```
By default, '/dev/video0' is the default webcam for laptops. So, different brand of cameras create permanent camera objects in the device input directory once connected to the laptop. If your camera is detected, it may give you logs like this:
```
/dev/video0  /dev/video1  /dev/video2  /dev/video3
```
You can delete multiple camera objetcs if you want in order to prevent yourself from being confused but if you don't want, you have to manually edit the launch file 'usb_cam-test.launch' inside 'usb_cam' package & change between choices of input devices '/dev/video1' or '/dev/video2' or '/dev/video3' to get the current camera stream from your usb webcam.    
## Visual Inspection of USB-Webcam on Ubuntu 18.04:
If our webcam has support in Ubuntu, we can visually inspect by opening the video device using a tool called 'Cheese'. Cheese is simply a webcam viewer. Enter the command 'cheese' in the Terminal. If it is not installed, you can install it using the following command:
```
sudo apt-get install cheese
```
If the driver and device are proper, you will get a video stream from the webcam. If you have multiple webcams (like in laptop) 'cheese' software will by default choose the webcam to show which has higher resolution.
## Interfacing the USB-Webcam with ROS:
Let's test the webcam using the 'usb_cam' package. The following command is used to launch the usb_cam nodes to display images from a webcam and publish ROS image topics at the same time:
```
cd ~/catkin_ws
source devel/setup.bash
roslaunch usb_cam usb_cam-test.launch
```
If everything works fine, you will get the image stream and logs in the Terminal. The image is displayed using the image_view package in ROS, which is subscribed to the topic called /usb_cam/image_raw.
