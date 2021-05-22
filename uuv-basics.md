# UUV Simülatör Genel Bakış ve Kurulum
## uuv_simulator
Unmanned Underwater Vehicle Simulator, ROV'ler  ve AUV'ler gibi insansız su altı araçlarının simülasyonu için gerekli olan Gazebo ve ROS'a dayalı çok kullanışlı bir paket setidir.

<!-- buraya foto eklemeliyiz bence @senceryazici -->

## Kurulum
1. Öncelikle `uuv_simulator` kurmadan önce `ros-<distro>-desktop-full` kurulu olduğundan emin olun. Rehberimiz `distro` olarak `melodic` kullanılarak oluşturulmuştur ve Gazebo uygulaması `ros-melodic-desktop-full` versiyonuyla birlikte gelmektedir. Kurulu değilse;

   ```
   sudo apt install ros-melodic-desktop-full
   ```

2. ```
   # uuv_simulator paket seti yüklenir
   sudo apt install ros-melodic-uuv-simulator
   
   # Gerekli paketler çalışma alanına klonlanıp yerele kopyalanır
   cd ~/catkin_ws/src
   git clone https://github.com/uuvsimulator/uuv_simulator.git
   sudo cp -r .uuv_simulator/uuv_assistants/templates/ /opt/ros/melodic/share/uuv_assistants/
   
   # Paket derlenir
   cd ~/catkin_ws
   catkin build -- uuv_assistants
   # Paket terminale tanıtılır ve kurulum tamamlanır
   source devel/setup.bash
   ```

[Sonraki adımda](create-vehicle.md) simülasyona aracımızı eklemeye başlayacağız.
