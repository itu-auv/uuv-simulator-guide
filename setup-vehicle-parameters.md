# Statik ve Dinamik Parametrelerin Belirlenmesi
[bir önceki adıma](create-vehicle.md) gidin.
[anasayfaya](index.md) dönün.

Bu bölümde araç üzerine etkiyen hidrostatik ve hidrodinamik etkilerin belirtildiği parametrelerin konfigürasyonu anlatılacaktır.


Önceki adımda `<robot_name>_description` paketini oluşturmuştuk, burada temelde bulunan dosya/klasörler:
```
.
├── CMakeLists.txt
├── launch
│   └── upload.launch
├── meshes
│   └── README.md
├── package.xml
├── robots
│   └── default.xacro *
└── urdf
    ├── actuators.xacro
    ├── base.xacro *
    ├── gazebo.xacro *
    ├── sensors.xacro
    └── snippets.xacro

```
şeklinde olacaktır. Bu parametreleri belirlenmesi esnasında `*` ile işaretli dosyaların üzerinde değişikler yapacağız / inceleyeceğiz.

- `robots/default.xacro` Robotun çağırıldığı dosya. `not:` Burada bir değişiklik yapmayacağız. İhtiyaç duyulduğunda 
  ```xml
  <!-- Aşağıdaki kısımda 1028 kg/m3 su yoğunluğu olarak verilmiştir.
  <fluid_density>1028.0</fluid_density>  
  ```
  değiştirilebilir.
- `urdf/base.xacro` 3D mesh, kütle, su yoğunluğu vs gibi etkilerin belirtildiği ve sensör/iticilere yönelik `.xacro` uzantılı dosyaların çağırıldığı dosya
- `urdf/gazebo.xacro` hidrodinamik/hidrostatik etkilere yönelik parametrelerin belirlendiği dosya

## Sonraki adımlar için
Sonraki adımlarda kullanacağımız sensörler ve iticilerde, takımımız tarafından hazırlanan bazı
snippet'lar bulunmaktadır. Bunlar sayesinde sensörün/iticinin ismini yazarak kolayca aracımıza 
ekleyebileceğiz. Öncelikle bunların bulunduğu paketi, aracımıza tanıtmamız gerekmektedir.
`urdf/base.xacro` içindeki, `include` bulunan bölümün sonuna aşağıdaki satır eklenerek bu işlem 
gerçekleştirilmiş olur.
```xml
<xacro:include filename="$(find ituauv_uuv_descriptions)/urdf/snippets.xacro"/> 
```
  

## Kütle, Ağırlık Merkezi, Yoğunluk ve Eylemsizlik Momenti
`urdf/base.xacro` içindeki, aşağıdaki bölümde
```xml
  <xacro:property name="mass" value="0"/>
  <!-- Center of gravity -->
  <xacro:property name="cog" value="0 0 0"/>
  <!-- Fluid density -->
  <xacro:property name="rho" value="1028"/>
```
`mass` ile gösterilen bölüme aracın kütlesi, `cog` ile gösterilen konuma ise aracın gövde merkezine göre ağırlık merkezi yazılır.
Eğer tasarımınız simetrik ve ağırlıklar eş şekilde dağıtılmış ise, ağırlık merkezi aracın hacimsel merkezindedir denebilir, böylece
çoğu zaman aracın gövde merkezi ağırlık merkezi ile eş kabul edilebilir. Bu durumda `cog` bölümü olduğu şekilde `0 0 0` olarak bırakılmalıdır.
`rho` parametresi için ise, `robots/default.xacro` içinde kullandığımız yoğunluk girilmelidir, ki bu 1028'dir.

Eylemsizlik momenti için `urdf/base.xacro` içindeki aşağıdaki bölümde
```xml
<inertial>
  <mass value="${mass}" />
  <origin xyz="${cog}" rpy="0 0 0"/>
  <inertia ixx="0" ixy="0" ixz="0"
           iyy="0" iyz="0"
           izz="0" />
</inertial>
```
`ixx, iyy, izz` kısmı doldurulmalıdır. Yine simetrik tasarımlarda, bu üç değer dışındaki diğer eylemsizlik momentleri genellikle `0`'a
çok yakın olduğundan ihmal edilebilir.
Örneğin araç x ekseni etrafında dönüyorken y ekseni yönünde bir eylemsizlik uygulanmıyorsa, `ixy` `0` olacaktır.
Eğer elinizde bulunuyorsa `3x3` boyutundaki inertia matrisini (ihmal edilebilecek diğer indisler ile birlikte) 

```xml
<inertial>
  <mass value="${mass}" />
  <origin xyz="${cog}" rpy="0 0 0"/>
  <inertia ixx="0" ixy="0" ixz="0"
           iyx="0" iyy="0" iyz="0"
           izx="0" izy="0" izz="0" />
</inertial>
```
şeklinde girebilirsiniz.

Yine `urdf/base.xacro` içindeki 

```xml
<xacro:property name="visual_mesh_file" value="file://$(find <robot_name>_description)/meshes/vehicle.dae"/>


<xacro:property name="collision_mesh_file" value="file://$(find <robot_name>_description)/meshes/vehicle.stl"/>
```
satırlarında `visual_mesh_file`(Görsel 3D Model) ve `collision_mesh_file`(Çarpışma geometrisi, visual_mesh_file ile aynı olabilir.)
dosyalarının dosya yolu belirtilmektedir. Burada, `meshes/` klasörü altında,
 - Aracın görsel 3D modelinin bulunduğu `.dae` uzantılı bir dosya 
 - Aracın çarpışma yüzeylerini belirten 3D Model `.dae ve ya .stl` uzantılı bir dosya belirtilmelidir. (Bu görsel model ile aynı olabilir)

`not:` `.dae` uzantılı 3D model için Blender programı kullanılarak export alınabilir.
`not:` Görsel ve çarpışma modelinde kullanılacak 3D modelde, mümkün olduğunca az ayrıntıya girilmesi, simülatörün bilgisayarınızı
yormaması açısından kritik önem taşıyabilir. 
`not:` Görsel modelde, aracın üzerindeki itici, pervane ve sensörleri eklemenize gerek olmayabilir. İleriki adımlarda göstereceğimiz sensör ve
itici konfigürasyonlarında, sizler için hazırladığımız 3D modeli bulunan sensör/iticiler kullanılmaktadır. Bunları kendi sensörleriniz için yeniden 
tanımlasanız dahi, 3D modellerini orada belirtebilirsiniz.


## Hidrodinamik Etkiler
`urdf/gazebo.xacro` altındaki,
```xml
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro">
    <!-- Yüzerlik merkezi koordinatları -->
    <xacro:property name="<robot_name>_cob" value="0 0 0"/>

    <!-- Aracın hacmi m^3 -->
    <xacro:property name="<robot_name>_volume" value="0"/>

    <!-- Aracı içine alan en küçük kutunun ölçüleri, X, Y, Z uzunluğu -->
    <xacro:property name="<robot_name>_length" value="0"/>
    <xacro:property name="<robot_name>_width"  value="0"/>
    <xacro:property name="<robot_name>_height" value="0"/>

    <xacro:macro name="<robot_name>_hydro_model" params="namespace">
      <link name="${namespace}/base_link">
        <neutrally_buoyant>0</neutrally_buoyant>
        <volume>${<robot_name>_volume}</volume>
        <box>
          <width>${<robot_name>_width}</width>
          <length>${<robot_name>_length}</length>
          <height>${<robot_name>_height}</height>
        </box>
        <center_of_buoyancy>${<robot_name>_cob}</center_of_buoyancy>
        
        <!-- 1) Fossen's equation of motion -->
        <hydrodynamic_model>
          <type>fossen</type>
          <added_mass>
            0 0 0 0 0 0
            0 0 0 0 0 0
            0 0 0 0 0 0
            0 0 0 0 0 0
            0 0 0 0 0 0
            0 0 0 0 0 0
          </added_mass>
          
          <linear_damping>
            0 0 0 0 0 0
          </linear_damping>
          
          <quadratic_damping>
            0 0 0 0 0 0
          </quadratic_damping>
        </hydrodynamic_model>
      </link>
    </xacro:macro>
</robot>
```

Bu bölümde, aracın hacmi, `volume` ile gösterilen bölümde, yüzerlik merkezi ise `cob` ile gösterilen bölümde belirtilmiştir.
Ayrıca giriş bölümünde ifade ettiğimiz, Added mass, linear damping ve quadratic damping parametreleri de burada belirtilebilir.

Takımımızın Turkuaz aracı için bu parametreler aşağıdaki gibidir. Bir CFD analiz programı ile bu parametreleri belirleyene kadar
aşağıdaki parametreleri kullanabilirsiniz, nitekim ortaya çıkan araçlar düşük hızlarda ve birbirlerine benzer geometride olduğundan
bu parametreler arasındaki farklılık ihmal edilebilir. (Aracın tasarımına göre bu durum değişmektedir. İhmal edilemeyecek düzeyde 
farklılıklar da olabilir.)

```yml
Added Mass:

10.7727 0 0 0 0 0 
0 10.7727 0 0 0 0
0 0 49.7679 0 0 0
0 0 0 1.0092 0 0
0 0 0 0 1.0092 0
0  0  0  0  0  0

L. Damping:
-19.5909 -19.5909 -50.5595 -13.3040 -13.2181 -1.1559

Q. Damping:
-5.1386 -5.1386 -26.1105 0 0 0
```

[Bir sonraki adımda](setup-thrusters.md) iticilerin konfigürasyonu işlenecektir.

