# Teknofest Simülasyon Ortamı Kurulumu ve Çalıştırılması
Bu adımda simülasyon ortamının kurulumu & çalıştırılması anlatılacaktır.


## Simülasyon Ortamının Oluşturulması
Teknofest Sualtı yarışması için hazırladığımız simülasyon ortamını indirmek için aşağıdakı komutlar çalıştırılmalıdır
```sh
# catkin workspace oluşturmadıysanız oluşturun
mkdir -p ~/catkin_ws/src

# oluşturduğunuz workspace'in source klasörüne gidin
cd ~/catkin_ws/src

# Teknofest simülasyon ortamı için teknofest_simulator paketini klonlayalım.
git clone git@github.com:itu-auv/teknofest-simulator.git

# Catkin build komutu çalıştırılırken workspace içinde bulunulmalıdır,
# bu sebeple workspace klasörüne tekrar geri dönün
cd ~/catkin_ws

# Paketi derleyin
catkin build -- teknofest_simulator
```
Yukarıdaki işlemler tamamlandıktan sonra `source ~/catkin_ws/devel/setup.bash` komutunu çalıştırarak derlemiş olduğumuz paketi
bulunduğunuz terminalin environment'ında tanımlamış olduk.

Simülasyon ortamını çalıştırmak için `source` işlemi yapılan terminalde aşağıdaki komut çalıştırılır.

```sh
roslaunch teknofest_simulator start.launch
```

## Simülasyon Ortamına Aracın Yüklenmesi
Bu adımda daha önceki adımlarda test amaçlı oluşturmuş olduğumuz aracı yüklemek için aşağıdaki komut çağırılır.
Yine önceki adımlarda da olduğu gibi `<robot_name>` kısmı, aracınıza verdiğiniz isim ile değiştirilmelidir.

```sh
roslaunch <robot_name>_description upload.launch
```
komutu çalıştırılır. Bu durumda Gazebo'da tasarlanmış aracın görülüyor olması gerekir.

Tüm sistemlerin fonksiyonel olduğuna dair bir genel bakış yapmak için 
```sh
rostopic list
```
komutu çalıştırılabilir. Burada ROS ağı üzerinde yayımlanmakta olan topic'ler görülebilir. 
Bu listede örnek olarak aşağıdaki bir kaç topic aranabilir, `rostopic echo <topic_ismi>` ile de 
istenilen topic'deki veri akışı gözlemlenebilir.

```
/<robot_name>/sensors/imu/data_raw
/<robot_name>/sensors/imu/mag
/<robot_name>/cameras/cam_<camera_name>/image_raw
/<robot_name>/sensors/pressure
/<robot_name>/thrusters/0/input
...
/<robot_name>/thrusters/7/input
```

## Simülasyon Ortamından Odometri (Ground Truth) Bilgisi Elde Edilmesi
Sensörlerden elde edilen bilgilerin yanısıra, geliştirilen algoritmaları test edebilmek ve aracın gerçek 
konumunu görebilmek amacıyla simülatör tarafından  ground truth odometri bilgisi `/<robot_name>/pose_gt` 
topic'inde `nav_msgs/Odometry` mesaj tipinde yayımlanmaktadır. Bu bilgi `<robot_name>_description/urdf/sensors.xacro`
dosyası içerisindeki 
```xml
<default_pose_3d_macro namespace="${namespace}" parent_link="${namespace}/base_link" inertial_reference_frame="${inertial_reference_frame}" />
```
satırında eklenen plugin ile sağlanmaktadır.

`not:` Bu bilgi simülatör tarafından sağlanmakta olup, gerçek yarışma ortamında
bu bilgilerin sağlanmayacağını unutmayın.


[Sonraki adımda](thruster-commanding.md) simülasyon ortamında iticilerin kontrol edilmesi işlenecektir. 

