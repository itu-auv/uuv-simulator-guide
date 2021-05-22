# Sensörlerden Veri Alınması
[bir önceki adıma](thruster-commanding.md) gidin.
[anasayfaya](index.md) dönün.


`uuv_simulator` üzerinde tanımladığınız araçta yerleştirilen sensörleri, `sensors.xacro` içinde gerekli parametrelerle tanımlamıştık. Bu tanımlamalar yapılırken topic parametresi örnek olarak şekildeki gibi verilmektedir

```xml
<bluerobotics_bar30 namespace="${namespace}" topic="sensors/pressure" parent_link="${namespace}/base_link" suffix="">
  <origin xyz="0.02666 -0.019524 -0.084" rpy="0 ${1.0*pi} 0" />
</bluerobotics_bar30>
```

Bu tanımlamada, sensörden elde edilen basınç değeri `/<robot_name>/sensors/pressure` topic'i altında erişilebilecektir. Bu örnekteki basınç sensörü için belirtilen topic'de ROS [sensor_msgs/FluidPressure](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/FluidPressure.html) tipinde bir mesaj yayımlanacaktır.

## Basınç Sensörü
```yml
topic: `/<robot_name>/sensors/pressure`
mesaj tipi: sensor_msgs/FluidPressure
Açıklama: Basınç verisi
```
 - `FluidPressure.msg`
   - **std_msgs/Header** `header` # Mesajın timestamp ve frame bilgilerini taşıyan header kısmı
   - **float64** `fluid_pressure` # Basınç değeri (KPa)
   - **float64** `variance` # [Varyans](https://tr.wikipedia.org/wiki/Varyans)

## IMU
```xml
<xsens_mti_g710 namespace="${namespace}" topic="sensors/imu/data_raw" topic_mag="sensors/imu/mag" parent_link="${namespace}/base_link">
  <origin xyz="0.13823 0 -0.042273" rpy="0 0 0" />
</xsens_mti_g710>
```
IMU için elde edilen veriler birden fazla olduğundan dolayı, genellikle `/..../imu/` altında bir kaç adet topic bulunur. Örneğin `/<robot_name>/sensors/imu/data` imu'dan elde edilen [sensor_msgs/Imu](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/Imu.html) mesaj tipindeki veriyi ifade ederken, bununla beraber başka topic'lerde bulunmaktadır. Belirlenmiş bu ortak topic kümesi altında `.../imu/mag` [sensor_msgs/MagneticField](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/MagneticField.html) mesaj tipinde manyetometre bilgisi de bulunmakta ve tüm bunlar `imu/` prefix'i altında bulunmaktadır. Bu kapsamda, 
```yml
topic: `/<robot_name>/sensors/imu/data_raw` # IMU'dan gelen ivme, oryantasyon ve açısal hız bilgisi
mesaj tipi: sensor_msgs/Imu
Açıklama: İvme, Oryantasyon ve Açısal Hız


topic: `/<robot_name>/sensors/imu/mag` # IMU'nun içindeki manyetometre tarafından okunan manyetik alan bilgisi
mesaj tipi: sensor_msgs/MagneticField
Açıklama: Manyetik alan
```
 `not:` Burada `data` yerine `data_raw` isimlendirilmesi, sensorden gelen işlenmemiş (ham) veriyi ayırt etmek için yapılmaktadır. Bir filtre kullanılarak Bu `data_raw` ve `mag` topic'leri filtreden geçirilerek çıkış `data` topic'ine yazılır.
 Böylece
 
 ```
/<robot_name>/sensors/imu/
--------------------------data_raw # Ham veri
--------------------------mag # Manyetometre
--------------------------data # Filtrelenmiş / Gürültüsü bastırılmış ve Fuse edilmiş IMU bilgisi (İşlenmiş, bknz: imu_filter_madgwick)
```


## Sonar
Mesafe ölçümü için kullanılan bu sonar için 
```xml
<bluerobotics_ping_sonar namespace="${namespace}" suffix="_right" parent_link="${namespace}/base_link" topic="sensors/sonar_right">
  <origin xyz="0.1 -0.15 -0.08" rpy="0 0 ${-0.5*pi}" />
</bluerobotics_ping_sonar>
```
`/<robot_name>/sensors/sonar_right` topic'inde [sensor_msgs/Range](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/Range.html) mesaj tipinde uzaklık bilgisi yayımlanmaktadır. Burada `Range` mesajı altındaki alanların açıklaması için [sensor_msgs/Range](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/Range.html) tıklayınız.


## Python2.7 ile örnek bir subscribe işlemi
```py
#!/usr/bin/env python
# package_name.msg import MessageName Şeklinde import ediyoruz
# Bu örnekte paket ismimiz sensor_msgs, mesaj tipimiz ise FluidPressure
from sensor_msgs.msg import FluidPressure

# rospy'i import ediyoruz
import rospy

class PressureSubscriberNode:
    def __init__(self):
        rospy.loginfo("Starting Pressure Subscriber Node.")
        self.latest_pressure_value = 0.0
        # Subscriber oluşturuyoruz, Her yeni mesaj geldiğinde, self.callback fonksiyonu çalışacak 
        rospy.Subscriber("/<robot_name>/sensors/pressure", FluidPressure, self.callback)

    def callback(self, msg):
        self.latest_pressure_value = msg.fluid_pressure
        rospy.loginfo("Water pressure (KPa): {}".format(msg.fluid_pressure))

if __name__ == "__main__":
    # Tüm ROS bağımlı işlemleri gerçekleştirmeden önce, 
    # Node'u initalize etmek gerekir.
    rospy.init_node("pressure_sub_node")
    node = PressureSubscriberNode()
    
    # Thread'i burada canlı tutmak gerekmektedir.
    rospy.spin()
```

[Bir sonraki adımda](summary.md) bu kılavuza yönelik özeti inceleyeceğiz
