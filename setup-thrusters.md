# İticilerin Konfigürasyonu
[bir önceki adıma](setup-vehicle-parameters.md) gidin.
[anasayfaya](index.md) dönün.

`uuv_simulator`'de iticilerin oluşturulması ve konfigürasyonu için,

`/<robot_name>/urdf/actuators.xacro` dosyasında:

```xml
<robot xmlns:xacro="http://www.ros.org/wiki/xacro">
  <!-- 
  İtici:       Z Ekseni 0 numaralı itici 
  origin:      Yerleşim, XYZ ve Roll Pitch Yaw olarak, base_link 
               (gövde merkezi)'e göre konum
  thruster_id: İtici numarası, 0
  Açıklama:    Buradaki "t100_thruster" ituauv_uuv_descriptions 
               paketinde snippet olarak tanımlanmıştır. 
               "t100_thruster" veya "t200_thruster" kullanılabilir
               Bunlar BlueRobotics T100 ve T200 iticileri için
               hazırlanmıştır. Farklı bir itici kullanıyorsanız
               Issues bölümünde issue açıp belirtebilirsiniz.
  -->
  <xacro:t100_thruster namespace="${namespace}" thruster_id="0">
    <origin xyz="0.26 0.24 0.013" rpy="0 ${0.5*pi} 0"/>
  </xacro:t100_thruster>
  
  <!-- 
  İtici:       Z Ekseni 1 numaralı itici 
  origin:      Yerleşim, XYZ ve Roll Pitch Yaw olarak, base_link 
               (gövde merkezi)'e göre konum
  thruster_id: İtici numarası, 1
  Açıklama:    Buradaki "t100_thruster" ituauv_uuv_descriptions 
               paketinde snippet olarak tanımlanmıştır. 
               "t100_thruster" veya "t200_thruster" kullanılabilir
               Bunlar BlueRobotics T100 ve T200 iticileri için
               hazırlanmıştır. Farklı bir itici kullanıyorsanız
               Issues bölümünde issue açıp belirtebilirsiniz.
  -->
  <xacro:t100_thruster namespace="${namespace}" thruster_id="1">
    <origin xyz="0.26 -0.24 0.013" rpy="0 ${0.5*pi} 0"/>
  </xacro:t100_thruster>
  
  <!-- 
  İtici:       Z Ekseni 2 numaralı itici 
  origin:      Yerleşim, XYZ ve Roll Pitch Yaw olarak, base_link 
               (gövde merkezi)'e göre konum
  thruster_id: İtici numarası, 2
  Açıklama:    Buradaki "t100_thruster" ituauv_uuv_descriptions 
               paketinde snippet olarak tanımlanmıştır. 
               "t100_thruster" veya "t200_thruster" kullanılabilir
               Bunlar BlueRobotics T100 ve T200 iticileri için
               hazırlanmıştır. Farklı bir itici kullanıyorsanız
               Issues bölümünde issue açıp belirtebilirsiniz.
  -->
  <xacro:t100_thruster namespace="${namespace}" thruster_id="2">
    <origin xyz="-0.25 0.24 0.013" rpy="0 ${0.5*pi} 0"/>
  </xacro:t100_thruster>
  
  <!-- 
  İtici:       Z Ekseni 3 numaralı itici 
  origin:      Yerleşim, XYZ ve Roll Pitch Yaw olarak, base_link 
               (gövde merkezi)'e göre konum
  thruster_id: İtici numarası, 3
  Açıklama:    Buradaki "t100_thruster" ituauv_uuv_descriptions 
               paketinde snippet olarak tanımlanmıştır. 
               "t100_thruster" veya "t200_thruster" kullanılabilir
               Bunlar BlueRobotics T100 ve T200 iticileri için
               hazırlanmıştır. Farklı bir itici kullanıyorsanız
               Issues bölümünde issue açıp belirtebilirsiniz.
  -->
  <xacro:t100_thruster namespace="${namespace}" thruster_id="3">
    <origin xyz="-0.25 -0.24 0.013" rpy="0 ${0.5*pi} 0"/>
  </xacro:t100_thruster>


  <!-- 
  İtici:       X Ekseni 4 numaralı itici 
  origin:      Yerleşim, XYZ ve Roll Pitch Yaw olarak, base_link 
               (gövde merkezi)'e göre konum
  thruster_id: İtici numarası, 4
  Açıklama:    Buradaki "t100_thruster" ituauv_uuv_descriptions 
               paketinde snippet olarak tanımlanmıştır. 
               "t100_thruster" veya "t200_thruster" kullanılabilir
               Bunlar BlueRobotics T100 ve T200 iticileri için
               hazırlanmıştır. Farklı bir itici kullanıyorsanız
               Issues bölümünde issue açıp belirtebilirsiniz.
  -->
  <xacro:t100_thruster namespace="${namespace}" thruster_id="4">
    <origin xyz="0.02508 0.24 -0.01" rpy="0 0 0"/>
  </xacro:t100_thruster>
  
  <!-- 
  İtici:       X Ekseni 5 numaralı itici 
  origin:      Yerleşim, XYZ ve Roll Pitch Yaw olarak, base_link 
               (gövde merkezi)'e göre konum
  thruster_id: İtici numarası, 5
  Açıklama:    Buradaki "t100_thruster" ituauv_uuv_descriptions 
               paketinde snippet olarak tanımlanmıştır. 
               "t100_thruster" veya "t200_thruster" kullanılabilir
               Bunlar BlueRobotics T100 ve T200 iticileri için
               hazırlanmıştır. Farklı bir itici kullanıyorsanız
               Issues bölümünde issue açıp belirtebilirsiniz.
  -->
  <xacro:t100_thruster namespace="${namespace}" thruster_id="5">
    <origin xyz="0.02508 -0.24 -0.01" rpy="0 0 0"/>
  </xacro:t100_thruster>
</robot>
```

Buradaki `origin` bloğunda yapılan konumlandırma, `base_link` (gövde) koordinat eksenine göre yapılmalıdır.

## İticiler
- `t100_thruster`: [Blue Robotics T100 Thruster](https://bluerobotics.com/store/retired/t100-thruster/)
- `t200_thruster`: [Blue Robotics T200 Thruster](https://bluerobotics.com/store/thrusters/t100-t200-thrusters/t200-thruster-r2-rp/)
[ituauv_uuv_descriptions](https://github.com/itu-auv/ituauv-uuv-descriptions) paketinde halihazırda kullanılabilecek bu iki itici bulunmaktadır.
Bunlar ilgili ürünün sitesinde yayımlanan PWM-KUVVET grafiğinden yararlanılarak oluşturulmuş itici modelleridir. İleriki adımlarda göreceğimiz
iticilerin kontrol edilmesine ilişkin giriş çıkış bağıntıları, burada tanımlanacak iticilere göre değişecektir. 

[Bir sonraki adımda](setup-sensors.md) sensörlerin oluşturulması ve konfigürasyonu işlenecektir.
