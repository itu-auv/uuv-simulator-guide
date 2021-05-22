# Yeni Araç Oluşturulması
[bir önceki adıma](uuv-basics.md) gidin.
[anasayfaya](index.md) dönün.


Bu bölümde `uuv_simulator` kullanarak yeni bir araç oluşturacağız. Burada araca ait bazı parametrelere
ve bilgilere sahip olunması gerekirken bazıları da opsiyonel şekilde belirtilecektir. Ölçümü zor olan 
bazı bilgiler için İTÜ AUV Takımı olarak Turkuaz aracımızın parametreleri bu bölümde verilecektir. 


## İsim Belirlemek!
İlk ve en önemli adım isim belirlemek. Hazırladığımız bu simülatör klavuzunun tamamında aracın ismi ve
bununla ilgili komutlarda geçen isim bölümleri `<robot_name>` olarak anılacaktır. Bu komutlardaki `<robot_name>`
kısmı, bu adımda belirleyeceğiniz isim ile değiştirilmelidir.

## İhtiyaç Olunan Bilgiler
Bahsedildiği gibi bu simülasyonun gerçekçi olabilmesi için bazı parametrelerin, araç tasarımının yapıldığı 
programlardan (Autodesk Fusion 360, Solidworks vb) elde edilmesi, çeşitli analiz programları kullanılarak 
(Ansys, OpenFoam vb) elde edilmesi ve test düzenekleri kurularak elde edilmesi gerekebilmektedir.


  - Kütle (Kg)
  - Hacim (m^3)
  - Ağırlık Merkezi (x,y,z)
  - Yüzerlik Merkezi (x,y,z)
  - Eylemsizlik Momenti <sup>1</sup>
  - Katma Kütle (Added Mass) <sup>2</sup>
  - Lineer ve İkinci Derece Sönümleme Matrisi <sup>3</sup>
  - İtici Konumları (x,y,z)
  - Sensör Konumları (x,y,z)
  - Aracın 3B Modeli (x,y,z)
  - Aracın Ölçüleri (Minimum kaplayan dikdörtgen prizma ayrıt uzunlukları)
    - `Örn:` `x: 0.6, y: 0.4, z: 0.5` 


  `1:` Eylemsizlik Momenti Matrisi
  ```
      | i_xx i_xy i_xz |
  I = | i_yx i_yy i_yz |
      | i_zx i_zy i_zz |
  ```
  `2:` Katma Kütle (Added Mass) Matrisi (Fossen, 1994)
  ```
       | A_11 A_12 A_13 A_14 A_15 A_16 |
       | A_21 A_22 A_23 A_24 A_25 A_26 |
  MA = | A_31 A_32 A_33 A_34 A_35 A_36 |
       | A_41 A_42 A_43 A_44 A_45 A_46 |
       | A_51 A_52 A_53 A_54 A_55 A_56 |
       | A_61 A_62 A_63 A_64 A_65 A_66 |
  ```
  Simetrik sistemler için `A_11, A_22, A_33, A_44, A_55, A_66` hariç sıfır kabul edilebilir.
  `3:` Lineer ve İkinci Derece Sönümleme (Damping) Matrisi
  ```
       | D1   0   0  0  0  0 |
       |  0  D2   0  0  0  0 |
  DL = |  0   0  D3  0  0  0 |
       |  0   0   0 D4  0  0 |
       |  0   0   0  0 D5  0 |
       |  0   0   0  0  0 D6 |
       
       | D1   0   0  0  0  0 |
       |  0  D2   0  0  0  0 |
  DQ = |  0   0  D3  0  0  0 |
       |  0   0   0 D4  0  0 |
       |  0   0   0  0 D5  0 |
       |  0   0   0  0  0 D6 |
  ```
 
Katma kütle ve sönumleme matrisleri için ilgili parametreler CFD ile elde edilmelidir.
Fakat bu zor bir işlem olduğundan, bu parametreler için **Turkuaz** aracımızın parametrelerini 
kullanabilirsiniz.


## Yeni Araç Oluşturma

`uuv_simulator` apt server üzerinden yayımlanan son versiyonunda, `uuv_assistants` paketinde bir [sorun](https://github.com/uuvsimulator/uuv_simulator/issues/385)
olduğundan dolayı, sadece aracı oluştururken, `uuv_simulator` kaynak kodunu bilgisayarımıza çekmemiz gerekmektedir.
Araç oluşturulduktan sonra bu kaynak kodu silebiliriz.

-  ```
   # uuv_simulator paket seti yüklenir
   sudo apt install ros-melodic-uuv-simulator
   
   
   # Eğer oluşturmadıysanız 
   mkdir -p ~/catkin_ws/src 
   
   # ~/catkin_ws/src dosya yoluna gidilir
   cd ~/catkin_ws/src
   
   # uuv_assistants için paket çalışma alanına klonlanır
   git clone https://github.com/uuvsimulator/uuv_simulator.git
   
   # Bahsedilen sorunun çözümüne yönelik aşağıdaki komut çalıştırılır.
   sudo cp -r uuv_simulator/uuv_assistants/templates/ /opt/ros/melodic/share/uuv_assistants/
   
   # sonrasında kaynak kod silinebilir
   rm -rf uuv_simulator
   ```

-  `uuv_simulator` kurulumu yapıldıktan sonra aşağıdaki kodla aracınızın tanım paketi oluşturulur.
   
   ```
   cd ~/catkin_ws/src
   rosrun uuv_assistants create_new_robot_model --robot_name <robot_name>
   ```

   *Örnek:* `rosrun uuv_assistants create_new_robot_model --robot_name test_robot`

   *Doğru çıktı şöyle olmalı:*

   ```
   username@pc:~/catkin_ws/src$ rosrun uuv_assistants create_new_robot_model --robot_name test_robot
   Create new catkin package for a UUV robot description
   	Robot name = test_robot
   	Catkin package name = test_robot_description
   Creating the catkin package...
   Created file test_robot_description/CMakeLists.txt
   Created file test_robot_description/package.xml
   Successfully created files in /home/eren/simulation_ws/src/test_robot_description. Please adjust the values in package.xml.
   Done!
   Creating folder=test_robot_description/launch
   Creating file:
   	test_robot_description/launch/upload.launch
   Creating folder=test_robot_description/meshes
   Creating file:
   	test_robot_description/meshes/README.md
   Creating folder=test_robot_description/urdf
   Creating file:
   	test_robot_description/urdf/gazebo.xacro
   Creating file:
   	test_robot_description/urdf/actuators.xacro
   Creating file:
   	test_robot_description/urdf/base.xacro
   Creating file:
   	test_robot_description/urdf/sensors.xacro
   Creating file:
   	test_robot_description/urdf/snippets.xacro
   Creating folder=test_robot_description/robots
   Creating file:
   	test_robot_description/robots/default.xacro
   Robot description package <test_robot_description> create successfully
   ```

   *Not:*

    - ```
      [rosrun] Couldn't find executable named create_new_robot_model below /opt/ros/melodic/share/uuv_assistants
      ```

      veya

    - ```
      Traceback (most recent call last):
        File "/opt/ros/melodic/lib/uuv_assistants/create_new_robot_model", line 59, in <module>
          for d in os.listdir(template_path):
      OSError: [Errno 2] No such file or directory: '/opt/ros/melodic/share/uuv_assistants/templates/robot_model'
      ```

      hatalarını alırsanız 2. adımı yaptığınızdan emin olun.


### Aracınızın tanım dosyası

`uuv_assistants` paketindeki `create_new_robot_model` scripti çalıştırıldıktan sonra `~/catkin_ws/src` klasörünün içinde `<robot_name>_description` dosyası oluşacaktır. Oluşturulan dosyanın içeriği aşağıdaki şekildedir.

```
.
├── CMakeLists.txt
├── launch
│   └── upload.launch
├── meshes
│   └── README.md
├── package.xml
├── robots
│   └── default.xacro
└── urdf
    ├── actuators.xacro
    ├── base.xacro
    ├── gazebo.xacro
    ├── sensors.xacro
    └── snippets.xacro
```

![chart_s](https://user-images.githubusercontent.com/84081125/118955306-3fe9f780-b967-11eb-9d5d-85313b591446.png)

[Bir sonraki adımda](setup-vehicle-parameters.md) statik ve dinamik parametrelerin konfigürasyonu işlenecektir.
