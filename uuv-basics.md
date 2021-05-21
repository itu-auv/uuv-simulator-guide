**Teknofest Simülasyon ortamını kullanmak için özetle yapmanız gerekenler** [detaylı anlatım için: [teknofest-simulator](https://github.com/itu-auv/teknofest-simulator/blob/main/README.md) ]

```
sudo apt install ros-melodic-uuv-simulator
```
<!-- look here @senceryazici -->
<!-- (@senceryazici) Bu hatayı ben almıştım ama biraz nadir bir hata (sanırım başka bir şeyin kurulumunu yarım bıraktığım içinmiş) istiyosan kaldıralım -->
- ​	`N: '/etc/apt/sources.list.d/' dizinindeki 'example-example.list.save' dosyası geçersiz bir dosya uzantısı olduğu için yok sayılıyor`  hatası verirse;

  `sudo sh -c "echo 'Dir::Ignore-Files-Silently:: \"(.save|.distupgrade)$\";' > /etc/apt/apt.conf.d/99ignoresave"`


```
# Workspace klasörü oluşturulur
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
git clone git@github.com:itu-auv/teknofest-simulator.git
cd ~/catkin_ws

# Paket derlenir
catkin build -- teknofest_simulator
# Kaynak yapılıp kurulum tamamlanır
source devel/setup.bash
```

```
roslaunch teknofest_simulator start.launch
```

