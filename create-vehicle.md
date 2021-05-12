# Yeni Araç Oluşturulması

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


<!-- TODO (aerenkaradag): Create new vehicle -->
## 
