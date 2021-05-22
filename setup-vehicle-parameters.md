# Statik ve Dinamik Parametrelerin Konfigürasyonu

##### Özel Kontrolör Paketi Oluşturma

İlk olarak bu uygulama için gerekli launch ve scripts dosyalarını içerecek yeni bir catkin paketi oluşturulur.Eğer sizin çalışma alanınız farklı şekilde adlandırılmışsa  *catkin_ws* adını kendi çalışma alanınızın ismiyle değiştirin.

```
cd ~/catkin_ws/src
catkin_create_pkg uuv_tutorial_dp_controller
```

Bu komut, daha sonra düzenlenecek olan yeni paket için gerekli dosyaları oluşturacaktır.Sonrasında launch dosyaları için bir launch klasörüne ve özel denetleyicinin uygulamasının depolanacağı script klasörüne ihtiyaç olacaktır.

```
cd ~/catkin_ws/src/uuv_tutorial_dp_controller
mkdir launch scripts
```

##### Kontrol Düğümünü Oluşturma

Scripts dosyasının içinde bir Python dosyayı oluşturulmalıdır.Burada bu dosyayı *tutorial_dp_controller.py* olarak adlandırdık.

```
touch scripts/tutorial_dp_controller.py
```

Bu komut aşağıda gösterilen `TutorialDPController` adındaki oldukça basit PID kontrolörünün uygulmasına sahip olan Pyhton dosyasını oluşturacaktır.
