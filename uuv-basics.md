# UUV Simülatör Genel Bakış ve Kurulum
[anasayfaya](index.md) dönün.


## uuv_simulator
Unmanned Underwater Vehicle Simulator, ROV'ler  ve AUV'ler gibi insansız su altı araçlarının simülasyonu için gerekli olan Gazebo ve ROS'a dayalı çok kullanışlı bir paket setidir.

<!-- buraya foto eklemeliyiz bence @senceryazici -->

## Kurulum
- Öncelikle `uuv_simulator` kurmadan önce `ros-<distro>-desktop-full` kurulu olduğundan emin olun. Bu kılavuzda `distro` olarak `melodic` kullanılmıştır ve Gazebo     simülatörü `ros-melodic-desktop-full` versiyonuyla birlikte gelmektedir. Kurulu değilse;
   
   ROS Melodic Kurulumu için [linkteki](http://wiki.ros.org/melodic/Installation/Ubuntu) yönergeleri takip edin

-  `uuv_simulator` kurulumu 
   ```
   # uuv_simulator paket seti yüklenir
   sudo apt-get install ros-melodic-uuv-simulator
   ```

[Sonraki adımda](create-vehicle.md) simülasyona aracımızı eklemeye başlayacağız.
