# Sensörlerin Konfigürasyonu
`uuv_simulator` üzerinde sensörleri konfigüre etmek için `/<robot_name>_description/urdf/sensors.xacro` dosyasına gidilir. Burada
örnek bir sensör yerleşimi için `/<robot_name>_description/urdf/sensors.xacro` içinde:
```xml
<robot xmlns:xacro="http://www.ros.org/wiki/xacro">
  <!-- 
  Sensör:      XSens MT-i G-710 IMU
  Yerleşim:    Origin bloğunda xyz ve roll pitch yaw olarak belirtilir. 
  topic:       Sensör bilgilerinin yayımlanacağı topic ismi. 
               Dikkat !!! Bu isimin başına <robot_name> gelecektir. 
  topic_mag:   Manyetometre bilgilerinin yayımlanacağı topic bilgileri.
  parent_link: Sensör koordinatlarının hangi koordinat takımı referans alınarak belirtildiği, frame. 
               ${namespace}/base_link için bu <robot_name>/base_link olacaktır. 
               Bu aracın merkez koordinatını ifade etmektedir.
  Açıklama:    Buradaki xsens_mti_g710, ituauv_uuv_descriptions paketi altında 
               snippet olarak önceden tanımlanmış ve ayarlanmıştır.
               Başka sensörlerin eklenmesi için, yeni snippet tanımlamak gerekir. 
               Bu tür eklemeler yakında yapılacaktır, eğer takımınız başka bir sensör 
               kullanıyorsa reponun issues bölümünde bir issue açarak belirtebilirsiniz. 
               En kısa sürede eklemeye çalışırız. Veya kendiniz ekleyip Pull Request açabilirsiniz
  -->
  <xsens_mti_g710 namespace="${namespace}" topic="sensors/imu/data_raw" topic_mag="sensors/imu/mag" parent_link="${namespace}/base_link">
    <origin xyz="0.13823 0 -0.042273" rpy="0 0 0" />
  </xsens_mti_g710>
  
  <!-- 
  Sensör:   Gerçek Konum Sensörü
  Açıklama: Simülasyon ortamından gerçek odometri bilgisini elde etmek için eklenir. 
            Temelde bu sensör, simülatörde bulunan gerçek base_link (gövde merkez) 
            konumunu world (referans, dünya, sabit nokta) koordinat takımına göre paylaşır. 
            Sensörlerden alacağınız verilerle elde edeceğiniz odometri/konum bilgisini 
            test etmek için kullanılabilir
  -->
  <default_pose_3d_macro namespace="${namespace}" parent_link="${namespace}/base_link" inertial_reference_frame="${inertial_reference_frame}" />
	
  <!-- 
  Sensör:       BlueRobotics Bar30 Derinlik sensörü
  Yerleşim:    Origin bloğunda xyz ve roll pitch yaw olarak belirtilir. 
  topic:       Sensörün derinlik bilgilerinin yayımlanacağı topic ismi. 
               Dikkat !!! Bu isimin başına <robot_name> gelecektir. 
  parent_link: Sensör koordinatlarının hangi koordinat takımı referans alınarak belirtildiği, frame. 
               ${namespace}/base_link için bu <robot_name>/base_link olacaktır. 
               Bu aracın merkez koordinatını ifade etmektedir.
  suffix:      Birden fazla basınç sensörü bulunması durumunda, pressure_front, pressure_back gibi 
               2 farklı isimlendirme yapmak için kullanılır, belirtilen örnekte suffix biri için "_front" 
               öteki için "_back" olacak şekilde "pressure" isminin sonuna gelen son eki ifade eder.
  Açıklama:    Buradaki bluerobotics_bar30, ituauv_uuv_descriptions paketi altında snippet olarak 
               önceden tanımlanmış ve ayarlanmıştır. Başka sensörlerin eklenmesi için, yeni snippet 
               tanımlamak gerekir. Bu tür eklemeler yakında yapılacaktır, eğer takımınız başka bir sensör 
               kullanıyorsa reponun issues bölümünde bir issue açarak belirtebilirsiniz. En kısa sürede
               eklemeye çalışırız. Veya kendiniz ekleyip Pull Request açabilirsiniz
  -->
  <bluerobotics_bar30 namespace="${namespace}" topic="sensors/pressure" parent_link="${namespace}/base_link" suffix="">
    <origin xyz="0.02666 -0.019524 -0.084" rpy="0 ${1.0*pi} 0" />
  </bluerobotics_bar30>
  
  <!-- 
  Sensör:      BlueRobotics Ping Sonar Uzaklık sensörü
  Yerleşim:    Origin bloğunda xyz ve roll pitch yaw olarak belirtilir. 
  topic:       Sensörün uzaklık bilgilerinin yayımlanacağı topic ismi. 
               Dikkat !!! Bu isimin başına <robot_name> gelecektir. 
  parent_link: Sensör koordinatlarının hangi koordinat takımı referans alınarak belirtildiği, frame. 
               ${namespace}/base_link için bu <robot_name>/base_link olacaktır. Bu aracın 
               merkez koordinatını ifade etmektedir.
  suffix:      Birden fazla basınç sensörü bulunması durumunda, sonar_front, pressure_back gibi 
               2 farklı isimlendirme yapmak için kullanılır, belirtilen örnekte suffix biri 
               için "_front" öteki için "_back" olacak şekilde "sonar" isminin sonuna gelen 
               son eki ifade eder.
  Açıklama:    Buradaki bluerobotics_ping_sonar, ituauv_uuv_descriptions paketi altında 
               snippet olarak önceden tanımlanmış ve ayarlanmıştır. Başka sensörlerin 
               eklenmesi için, yeni snippet tanımlamak gerekir. Bu tür eklemeler yakında 
               yapılacaktır, eğer takımınız başka bir sensör kullanıyorsa reponun issues 
               bölümünde bir issue açarak belirtebilirsiniz. En kısa sürede eklemeye çalışırız. 
               Veya kendiniz ekleyip Pull Request açabilirsiniz
  -->
  <bluerobotics_ping_sonar namespace="${namespace}" suffix="_bottom" parent_link="${namespace}/base_link" topic="sensors/sonar_bottom">
    <origin xyz="0.2967 -0.15 -0.28223" rpy="0 ${0.5*pi} 0" />
  </bluerobotics_ping_sonar>
</robot>
```

[Sonraki adımda](simulation.md) simülasyon ortamının oluşturulması işlenecektir
