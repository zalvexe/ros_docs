# MODUL ROS 1 - Noetic (PART 2 WOWOWWOO)  

Di part 2 ini akan bahas tentang custom messages :)  

## Custom Messages  
Jadi gini, misal kita pingin bikin publisher sendiri dengan message yang kita custom sendiri.   
contoh kita mau nerima data posisi robot(x,y,theta) dari microcontroller, nah kita bisa bikin message sendiri :]   

Untuk lebih memudahkan struktur file, kita bisa membuat messages sebagai 'package' sendiri yang berisi hanya folder /msg.  
di dalam folder /msg bisa kita isi dengan file ```pos.msg``` dengan isi

```msg
float32 Xpos
float32 Ypos
float32 ThetaPos
```
selanjutnya untuk CMakelists, beberapa konfigurasi yang digunakan yaitu  
```
cmake_minimum_required(VERSION 3.0.2)
project(nama package)

find_package(catkin REQUIRED COMPONENTS
  message_generation
  roscpp
  std_msgs
)

add_message_files(
  FILES
  Pos.msg
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
