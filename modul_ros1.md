# ROS 1 - Noetic Modul 2024

## Apa itu ROS??  
ROS (Robot Operating System) itu seperti sistem operasi buat robot, tapi bukan sistem operasi yang kita pakai sehari-hari di komputer. Bayangkan ROS sebagai toolkit besar yang menyediakan semua alat yang dibutuhkan robot untuk berpikir dan bertindak. Jadi, ROS itu semacam platform yang bikin robot bisa berinteraksi dengan dunia di sekelilingnya, ngolah data dari sensor, bergerak, dan berkomunikasi dengan perangkat lain.

![image](https://github.com/user-attachments/assets/0bc3f43f-cc55-486e-9354-8350a6690132)

ROS bikin hidup lebih mudah bagi para pengembang robot dengan menyediakan:
- **Library dan Tools**: Ini kaya kotak alat yang penuh dengan kode siap pakai untuk berbagai keperluan robot, seperti pengolahan gambar, kontrol motor, dan komunikasi.
- **Framework**: ROS memberikan struktur yang konsisten untuk membuat dan menjalankan berbagai aplikasi robot.
- **Komunikasi**: Dengan ROS, komponen robot yang berbeda bisa ngobrol satu sama lain dengan mudah, misalnya, sensor yang mengirim data ke prosesor yang kemudian memerintahkan motor.

Jadi, kalau kamu pengen bikin robot yang pintar, fleksibel, dan bisa berkomunikasi dengan komponen lain, ROS adalah alat yang bisa sangat membantu! :]   
     
     
     
## 1. Instalasi ROS 1 - Noetic
Untuk instalasi ROS1 Noetic, pastikan Operating System yang digunakan merupakan Ubuntu 20.04 oke :D   

### __Pra-instalasi :__      
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```  

Install curl dulu (kalo belum)   
```
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```    


### __Memulai instalasi :__   
Update sistem dulu man teman   
```
sudo apt update && sudo apt upgrade  
```   

Install full desktop :   
```
sudo apt install ros-noetic-desktop-full
```   


### __Environment setup (buat sourcing) :__    
(buat bash terminal) :
```
source /opt/ros/noetic/setup.bash
```   

(buat bash lain, e.g. zsh) : 
```
source /opt/ros/noetic/setup.bash
```   

Lanjut kita tambahin source ke bashrc atau zshrc :  
```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```  

```
source ~/.bashrc
```   
> untuk zsh bisa langsung diganti setup.zsh dan .zshrc


### __Install Dependencies untuk Keperluan Building Package :__   
```
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential

sudo apt install python3-rosdep  

sudo rosdep init

rosdep update
```   

## 2. Tes dulu dong ya   
Sebelum lanjut nih.. kita tes dulu instalasi ROS tadi pake program simple publisher & subscriber  

Bikin workspace & package duluu  
```
mkdir -p ~/tutorial_ws/src  

cd ~/tutorial_ws/   

catkin_make
``` 
> catkin_make itu buat build workspace yg kita bikin tadi

Lanjut kita sourcing, sourcing digunakan untuk set up environment yang digunakan di package dan executable file yang kita build nantinya  

```
source devel/setup.bash
```   
Kalo udah, kita masuk ke directory ```/src``` dan bikin node talker & listener   

```talker.cpp```
```cpp
#include "ros/ros.h"
#include "std_msgs/String.h"
#include <sstream>

int main(int argc, char **argv)
{
  ros::init(argc, argv, "talker");
  ros::NodeHandle n;
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
  ros::Rate loop_rate(1); // 1 Hz

  while (ros::ok())
  {
    std_msgs::String msg;
    std::stringstream ss;
    ss << "Hello ROS " << ros::Time::now();
    msg.data = ss.str();

    ROS_INFO("%s", msg.data.c_str());
    chatter_pub.publish(msg);

    ros::spinOnce();
    loop_rate.sleep();
  }

  return 0;
}
```

```listener.cpp```
```cpp
#include "ros/ros.h"
#include "std_msgs/String.h"

void chatterCallback(const std_msgs::String::ConstPtr& msg)
{
  ROS_INFO("I heard: [%s]", msg->data.c_str());
}

int main(int argc, char **argv)
{
  ros::init(argc, argv, "listener");
  ros::NodeHandle n;
  ros::Subscriber sub = n.subscribe("chatter", 1000, chatterCallback);
  ros::spin();

  return 0;
}
```
### Apa yang dilakukan program?  
Jadi, kedua program tersebut merupakan node talker dan node listener. Talker akan mempublish data berupa "Hello ROS" yang akan diterima oleh listener (kita ngambil informasi dari publisher lewat Callback function)  

Lanjut nih.. kita masuk ke file ```CMakeLists.txt```. CMakeLists ini berguna untuk mendefinisikan konfigurasi build yang digunakan di package kita   

```cmake
cmake_minimum_required(VERSION 3.0.2)
project(simple_demo_cpp)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
)

catkin_package()

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(talker src/talker.cpp)
target_link_libraries(talker ${catkin_LIBRARIES})

add_executable(listener src/listener.cpp)
target_link_libraries(listener ${catkin_LIBRARIES})
```
### Run program
Setelah 
Untuk run program ROS, kita akan perlu beberapa terminal :")     
terminal pertama kita pake buat Roscore   
```
roscore
```
lanjut.. terminal kedua kita run node talker   
```
ros run 

## 2. Membuat Workspace, Package, dan Node
  

__Membuat Package :__   
```catkin_create_pkg first_package std_msgs rospy roscpp```   
Di sini kita membuat package bernama 'first_package' dengan beberapa dependencies yaitu std_msgs, rospy, dan roscpp  

## 3. Build Workspace  
Untuk build workspace, maka perlu kembali ke directory workspace   
```cd ~/tutorial_ws/```   

dan jalankan build command   
```catkin_make```   

## 4. Publisher dan Subscriber

## 5. CMakeLists  
CMakeLists selalu berbeda dan disesuaikan dengan package yang kita buat. Umumnya, CMakeLists terdiri dari beberapa komponen yaitu :  
```cmake
cmake_minimum_required(VERSION 3.0.2)
project(rody1)
```
> dalam project() diisi dengan nama workspace
```cmake
add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
)
```
> find_package() diisi dengan package-package yang kita gunakan di workspace

```cmake
include_directories(
  ${catkin_INCLUDE_DIRS}
)
catkin_package(
  CATKIN_DEPENDS roscpp std_msgs
)
```
> dependencies pada pada catkin_package()

## 6. XML

## 7. Run ROS 1
## 7. Custom Messages
