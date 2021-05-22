# İticilerin Kontrol Edilmesi
[bir önceki adıma](simulation.md) gidin.
[anasayfaya](index.md) dönün.

Bu adımda simülasyon ortamında oluşturulan iticilerin kontrol edilmesi anlatılacaktır.

Simülasyon ortamı çalıştırıldığında, 
```
rostopic list
``` 
ile listelenen topic'ler arasında bulunan itici
topic'leri aşağıdaki gibidir

```
/<robot_name>/thrusters/<index>/dynamic_state_efficiency
/<robot_name>/thrusters/<index>/input
/<robot_name>/thrusters/<index>/is_on
/<robot_name>/thrusters/<index>/thrust
/<robot_name>/thrusters/<index>/thrust_efficiency
/<robot_name>/thrusters/<index>/thrust_wrench
```

Bu topic'ler her itici için oluşturulmakta ve `<index>` itici 
numarasını temsil etmektedir. Bu topic'lerden `.../input`, iticinin giriş sinyali için kullanılırken `.../thrust` ise çıkışını(uygulanan kuvveti) temsil etmektedir. Bu iticilerin giriş-çıkış ilişkisi, `<robot_name>_description/urdf/actuators.xacro` dosyası içerisinde tanımlanan aşağıdaki bölümde, 

```xml
<xacro:t100_thruster namespace="${namespace}" thruster_id="0">
    <origin xyz="0.1 0.1 0.0" rpy="0 ${0.5*pi} 0"/>
</xacro:t100_thruster>

....
<xacro:t100_thruster namespace="${namespace}" thruster_id="8">
    <origin xyz="0.7 0.2 -0.5" rpy="0 ${0.5*pi} 0"/>
</xacro:t100_thruster>
```
kullanılan iticiye aittir. Örneğin burada kullanılan itici `t100_thruster`, BlueRobotics'in T100 iticisini temsil etmektedir ve buna ait konfigürasyonlar `ituauv_uuv_descriptions` paketi altında yapılmıştır. İticinin girişi PWM iken çıkışı ise elde edilen kuvvettir ve bu bilgiler ürünün sitesinde verilen teknik dökümanlardan faydalanılarak elde edilmiştir. Kendi tasarladığınız iticiler için bu testleri kendiniz yapmanız gerekmektedir.

İticilerden bir tanesine komut vermek için `.../input` topic'ine veri göndermek yeterlidir bunun için basitçe,
```sh
rostopic pub /<robot_name>/thrusters/<index>/input uuv_gazebo_ros_plugins_msgs/FloatStamped "header:  
  seq: 0
  stamp:
    secs: 0
    nsecs: 0
  frame_id: ''
data: 1700.0"
```
çalıştırılabilir, böylece `<index>` numaralı itici, 1700 PWM girişi ile dönmeye başlayacak ve buna karşılık bir kuvvet üretecektir, örneğin T100 model iticiler için bu kuvvet değeri 10.3005 olacaktır.


## uuv_thruster_manager Kullanımı
Araç üstünde bulunan `<N>` adet iticinin bu şekilde kullanımı yerine, uuv simulator'ün `thruster_manager`'ı kullanılabilir.

Bunun için bir itici konfigürasyonu/parametreleri sağlamak gerekmektedir. `<robot_name>_description/config/t100_thruster_manager.yaml` içindeyken;

```yaml
thruster_manager:
  tf_prefix: <robot_name>
  base_link: base_link
  thruster_topic_prefix: thrusters/
  thruster_topic_suffix: /input
  thruster_frame_base: thruster_
  max_thrust: 24.0
  timeout: -1
  update_rate: 50
  conversion_fcn: custom
  conversion_fcn_params:
    input: [1100, 1110, 1120, 1130, 1140, 1150, 1160, 1170, 1180, 1190, 1200, 1210, 1220, 1230, 1240, 1250, 1260, 1270, 1280, 1290, 1300, 1310, 1320, 1330, 1340, 1350, 1360, 1370, 1380, 1390, 1400, 1410, 1420, 1430, 1440, 1450, 1460, 1470, 1480, 1490, 1500, 1510, 1520, 1530, 1540, 1550, 1560, 1570, 1580, 1590, 1600, 1610, 1620, 1630, 1640, 1650, 1660, 1670, 1680, 1690, 1700, 1710, 1720, 1730, 1740, 1750, 1760, 1770, 1780, 1790, 1800, 1810, 1820, 1830, 1840, 1850, 1860, 1870, 1880, 1890, 1900]
    output: [-17.33, -16.09, -15.46, -14.97, -14.13, -13.72, -12.97, -12.34, -11.85, -11.41, -10.83, -10.20, -9.71, -9.09, -8.38, -7.75, -7.39, -6.90, -6.55, -6.10, -5.70, -5.21, -4.63, -4.19, -3.92, -3.61, -3.20, -2.67, -2.27, -1.96, -1.64, -1.38, -1.02, -0.71, -0.49, -0.31, -0.13, -0.08, 0.00, 0.00, 0.00, 0.00, 0.00, 0.08, 0.35, 0.62, 1.02, 1.42, 1.91, 2.49, 3.03, 3.61, 4.23, 4.72, 5.34, 6.01, 6.73, 7.22, 7.88, 8.64, 9.45, 10.38, 11.09, 11.63, 12.30, 12.79, 13.59, 14.62, 15.46, 16.22, 16.93, 17.87, 19.21, 20.05, 20.59, 21.39, 22.19, 22.77, 23.71, 24.38, 24.69]
```

Buradaki `max_thrust`, iticinin sağlayabileceki maksimum kuvveti, `timeout`, iticiye belirli bir süre boyunca herhangi bir komut ulaşmaması durumunda iticinin kapatılması için gereken zaman aşımı süresini, `update_rate` ise, iticinin `.../input` topic'ine hangi frekansta yayın yapılacağını belirtmektedir. Bir çok ESC, 50Hz'lik bir frekansta kontrol edilmekle beraber bazıları 200 bazıları ise 490Hz'de kontrol edilmektedir. `timeout` parametresinin `-1` yapılması durumunda herhangi bir zaman aşımı olmayacağını dolayısıyla son verilen komutun sonsuza dek thruster'a iletilmeye devam edileceğini göstermektedir. `conversion_fcn_params` altında `input/output (giriş/çıkış)` ilişkisi verilmelidir.



Yukarıdaki parametreler iticiye ait parametreler iken bir de `Thruster Allocation Matrix(TAM)` ve ya `İtici Yerleşim Matrisi (İYM)` belirtilmelidir, bunun için `<robot_name>_description/config/tam.yaml` içerisinde, `<N>` itici sayısını temsil etmek üzere
`6xN` boyutunda bir matris tanımlanmalıdır. 

`N=6` olan bir sistem için örnek bir yerleşim matrisi aşağıda verilmiştir.

```yaml
tam:
  #   Th0    Th1    Th2    Th3    Th4   Th5  
  - [ 0.0 ,  0.0 ,  0.0 ,  0.0 ,  0.5,  0.5] # X Axis
  - [ 0.0 ,  0.0 ,  0.0 ,  0.0 ,  0.0,  0.0] # Y Axis
  - [ 0.25,  0.25,  0.25,  0.25,  0.0,  0.0] # Z Axis
  - [ 0.25, -0.25,  0.25, -0.25,  0.0,  0.0] # Rot X Axis
  - [-1.0 , -1.0 ,  1.0 ,  1.0 ,  0.0,  0.0] # Rot Y Axis
  - [ 0.0 ,  0.0 ,  0.0 ,  0.0 , -1.0,  1.0] # Rot Z Axis

```

```
Üst Bakış (Top View)


    (.) 0                              (.) 1
                       
                       
                       
                       ^ +X
                       |
      (5)              |              (6)
     ^          +Y     | (.)+Z          ^
     |          <------O                |
                       
                       
                       
                       
     <------ l1 ------>
                       
                       
  
    (.) 2                              (.) 3

```
Yukarıdaki şekilde konumlandırılan iticiler, 
  - X ekseni için, oluşturulmak istenecek `<X> N` kuvveti, 5 ve 6 numaralı iticiler yarı yarıya bölüşecektir bu sebeple matrisin 1. satırı için 5. ve 6. sütunlar 0.5  0.5 verilmiştir  
    
  - Y ekseninde, bu dizayn için bir kuvvet oluşturulamadığından 2. satır tamamen 0 olarak belirlenmiştir.
  - Z ekseni için, oluşturulmak istenecek `<X> N` kuvveti, 0, 1, 2 ve 3 numaralı iticiler `1/4` oranla bölüşecektir bu sebeple matrisin 3. satırı için 1. 2. 3. ve 4. sütunlar 0.25 0.25 0.25 0.25 verilmiştir.
  - Matrisin 4 numaralı satırı x eksenindeki torku ifade
  etmektedir, burada gövde üzerinde oluşturulmak istenen `<T> Nm` tork için, 
    ```
    F0*l1 + F2*l1 - (-F1*l1 + -F3*l1) = Tx
    ```
    olacağından, `0.25/l1, -0.25/l1, 0.25/l1, -0.25/l1` olarak seçilir.
  - Matrisin 5 ve 6 numaralı satırlarıda, benzer şekilde tamamlanır.



### Thruster Manager Çalıştırılması

Launch dosyası aşağıdaki gibi ayarlanarak çalıştırılabilir

`<robot_name>_description)/launch/thruster_manager.launch:`
```xml
<launch>
  <include file="$(find uuv_thruster_manager)/launch/thruster_manager.launch">
    <arg name="uuv_name" value="<robot_name>" />
    <arg name="model_name" value="<robot_name>" />
    <arg name="output_dir" value="$(find <robot_name>_description)/config"/>
    <arg name="config_file" value="$(find <robot_name>_description)/config/t100_thruster_manager.yaml"/>
    <arg name="tam_file" value="$(find <robot_name>_description)/config/tam.yaml"/>
  </include>
</launch>
```

```sh
roslaunch <robot_name>_description thruster_manager.launch 
```
komutu ile `Thruster manager` çalışmaya başladığında, artık her iticiyi ayrı ayrı kontrol etmektense, gövde üzerine
uygulanacak kuvvet ve tork giriş olarak verilebilecek, `thruster manager` her iticinin ne kadar kuvvet üretmesi gerektiğini hesaplayacaktır ve buna karşi düşen girişi ise verecektir.

Bunun için `/<robot_name>/thruster_manager/input` topic'ine, `geometry_msgs/Wrench` mesaj tipinde bir mesaj göndermek yeterli olacaktır.

Örnek olarak x ekseninde (ileri) hareket etmek istenmesi durumunda,
```sh
rostopic pub /turquoise/thruster_manager/input geometry_msgs/Wrench "force:
  x: 1.0
  y: 0.0
  z: 0.0
torque:
  x: 0.0
  y: 0.0
  z: 0.0"
```
komutu çalıştırılabilir.

[Bir sonraki adımda](sensor-data.md) sensörlerden veri alınması anlatılacaktır.
