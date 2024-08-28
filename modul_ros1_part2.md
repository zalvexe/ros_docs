# MODUL ROS 1 - Noetic (PART 2 WOWOWWOO)  

Di part 2 ini akan bahas tentang custom messages :)  

## Custom Messages  
Jadi gini, misal kita pingin bikin publisher sendiri dengan message yang kita custom sendiri.   
contoh kita mau nerima data controller robot(velx,vely,angvel), nah kita bisa bikin message sendiri :]   

Untuk lebih memudahkan struktur file, kita bisa membuat messages sebagai 'package' sendiri yang berisi hanya folder /msg.  
di dalam folder /msg bisa kita isi dengan file ```Controller.msg``` dengan isi

```msg
float32 velx
float32 vely
float32 angvel
```
selanjutnya untuk CMakelists, beberapa konfigurasi yang digunakan yaitu  
```cmake
cmake_minimum_required(VERSION 3.0.2)
project(nama package)

find_package(catkin REQUIRED COMPONENTS
  message_generation
  roscpp
  std_msgs
)

add_message_files(
  FILES
  Controller.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
)


catkin_package(
  # INCLUDE_DIRS include
  CATKIN_DEPENDS message_runtime
)

include_directories(
include
  ${catkin_INCLUDE_DIRS}
)
```
selain CMakeLists, kita perlu mengatur beberapa konfigurasi pada xml sebagai berikut  
```xml
  <buildtool_depend>catkin</buildtool_depend>
  <build_depend>std_msgs</build_depend>
  <build_depend>message_generation</build_depend>
  <build_depend>message_runtime</build_depend>
  <build_depend>roscpp</build_depend>
  
  <build_export_depend>std_msgs</build_export_depend>

  <exec_depend>std_msgs</exec_depend>
  <exec_depend>message_runtime</exec_depend>
  <exec_depend>rospy</exec_depend>
```

## How to use it? :0   
Pada package utama kita, buat folder untuk setiap node (biar lebih rapi aja sih) dan isi dengan .cpp file untuk node nya   

Untuk publisher yang akan menggunakan custom message yang telah dibuat, dapat diterapkan sebagai berikut   
```cpp
#include "package_messages/Controller.h"
```

Lalu kita deklarasikan Publisher
```cpp
ros::Publisher pub_pos;
```

Selanjutnya kita deklarasikan messages yang dipakai  
```cpp
package_messages::Controller Controller_msg;
```

Pada main() kita buat publisher  
```cpp
pub_controller = nh.advertise<package_messages::Controller>("Controller",10); //nh = nodehandle
```

Selanjutnya untuk node subscriber, kita bisa memanggil publisher dengan Callback()  
kita include dulu custom message
```cpp
#include "package_messages/Controller.h"
```

Selanjutnya kita deklarasi subscriber  
```cpp
ros::Subscriber sub_controller;
```

Selanjutnya kita panggil function callback  
```cpp
void controllerCallback(const package_messages::Controller::ConstPtr &Controller_msg);
```

Kemudian pada main() kita bisa tuliskan dengan   
```cpp
sub_controller = nh.subscribe("Controller", 10, controllerCallback); // Subscribe to Controller topic
```





