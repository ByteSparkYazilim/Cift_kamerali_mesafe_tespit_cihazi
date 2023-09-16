# Çift Kameralı Mesafe Tespit Cihazı

##### Bu çalışma, ByteSpark Robotik firmasının projelerinde kullanılmak üzere ihtiyaç duyulan çift kameralı bilgisayar görülü mesafe tespit işlemleri için, EE. Müh. Hazal Kara tarafından hazırlanmıştır.

##### Robotik sektöründe, insansız araçlar alanında ve otomasyon sistemleri alanında görüntü işleme birimleri oldukça sık kullanılmaktadır. Bilgisayar görülü matematiksel işlem birimleri sayesinde, dış dünya üzerinde veri sınıflandırmaları ve analizleri gerçekleştirilebilmektedir. Bahsi geçen sektörlerde, dış dünyada etki-tepki döngüsünde bulunacak teknolojik unsurların, çalışma gerçekleştirdiği bölgesel uzaydaki konumunu bilmesi önem arz etmektedir. Örneğin 3 boyutlu yazıcılar, step motorlar aracılığı ile doğrusal eksende hareket ettikleri mesafeleri veya açıları, limit switchler yardımı ile bilebilmektedirler. Evlerimizde kullandığımız robot süpürgeler, üzerlerinde bulunan lidar sensörü sayesinde fiziksel çevre ile etkileşime girebilmekte ve konumunu saptayabilmektedir. Stereo kamera teknolojisi ise, bilgisayar görüsü sayesinde, görüntüde tespit edilen bir noktanın, daha önceden belirlenen bir sabit noktaya göre uzaklığını elde etmemizi sağlamaktadır. 2 kameradan alınan görüntü farklarından ve geometrinin doğasından yola çıkılarak, tespit edilen nesnenin uzaklığı hesaplanabilmektedir.

##### Bu çalışmada, piyasada yer alan yüksek maliyetli ve dışa bağımlı çift kamera teknolojisi ile elde edilebilecek veriler, standart bir bilgisayar kamerası kullanılarak 10^-6 metre hassasiyetle hesaplanabilmiştir.

##### Bir cismin kameraya olan mesafesini ölçmek için iki yöntem vardır. Bunlardan ilki Aktif ölçümdür. Aktif ölçüm; lazer, kızılötesi, ses dalgaları sinyalleri kullanılarak sensörlerden alınan veriler ile yapılan mesafe ölçümdür. Burada yalnızca kullanılan sensör ve algılayıcıya olan uzaklık bilinir. Bir diğeri Pasif ölçüm; herhangi bir sinyal göndermeden mevcut bilgi değerlendirilir ve aynı zamanda mesafe ile birlikte cismin kamera ile arasındaki geometrisi ile 3 boyutlu haritası da çıkarılabilir. Bu projede pasif ölçüm yöntemlerinden biri olan iki kameradan elde edilen verilerin birleştirilerek mesafe hesabını yapan Stereo Görüntüleme kullanılacaktır.

### 1.STEREO GÖRÜŞ NEDİR?

##### Stereo Görüş, aynı düzlemdeki iki kameradan cismin görüntüsünü alır ve konumunu çıkarır. Cisimler arasındaki mesafe kamera ile nesne arasındaki mesafe ile ters orantılıdır. Bunu insan gözünün derinlik ve mesafe algısının tekniğine benzetebiliriz. Stereo görüntüleme tekniği iki kameradan alınan görüntüleri düzelterek ve bazı işlemlerden geçirerek birleştirir. Bu işlem sırası yüzeysel olarak şöyledir: 

1. Obje tespiti yapılır.
2. İki kameradan alınan görüntü birleştirme yapılır.
3. Eşitsizlik Haritası çıkarılır.
4. Cismin kamera ile derinlik ve mesafe tespiti yapılır.
5. Yer değiştirme ve kamera kalibrasyonları kullanılarak 3 boyutlu yapılandırma yapılır.

##### Burada eşitsizlik haritası yüksek önem arz etmektedir. Yüksek yer değişimlerinin olduğu noktalar, cismin kameraya daha yakın olduğunu ifade eder. Düşük eşitsizlik ise kameradan daha uzak olduğunu ifade eder. Stereo görüntüleme tekniği, gerçek zamanlı mesafe tespiti, ölçüm doğruluğunu arttırmak gibi amaçlarla ve robotik, trafik sistemleri, akıllı şehir uygulamaları, robotik endüstrisinde kullanılır. 

##### Projede kullanılan stereo kamera düzeneği aşağıda verilmiştir

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/3f316204-da05-4cf0-80ec-d9402b26a65e)

### 2. Kamera Çalışma Prensibi

##### Kameralar görüntüden yansıyan ışığı mercek ve objektiften yararlanarak bir düzlemde toplar. İnsandagörme olayı da çevreden yansıyan ışık ışınları göze gelerek retinada toplanır. Bir kameranın nasıl çalıştığını anlamak için iğne deliği kamerasını ele alırsak, modelde sadece delikten geçen ışık ışınları yakalanır ve ters olarak yapılandırılır. Bu prensip aşağıda görülmektedir.

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/185f28b0-cb81-47f0-8194-91b462f806c8)

#### 2.1. LENSİN BOZULMASI 

##### Bu ilke hızlı pozlamada ışığı yakalamak için yeterli değildir. Mercekler, tek bir yerde daha fazla ışık ışınını toplamak için kullanılır. Ancak bu durum lensin bozulmasına yol açar. İki farklı bozulma türü vardır: 2.1.1.Radyal Bozulma: Merceğin şeklinden kaynaklanan bozulmadır. Optik merkezde radyal distorsiyon yoktur ancak kenarlardan uzaklaştıkça kademeli olarak artar. Taylor açılımı ile bu bozulma düzeltilebilir. Burada r, üçüncü terime kadar yapılır. X ve y görüntü düzlemindeki noktaların koordinatlarıdır.


![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/25eaab85-e075-4e0a-a4a5-7262e4c1b801)

##### 2.1.2. Teğetsel Bozulma: Kameranın geometrisinden kaynaklı bozulmadır. Mercek görüntü düzlemine tam olarak paralel olarak oluşturulmadığı için oluşur. Bunu düzeltmek için formüle p1 ve p2 olmak üzere iki parametre eklenir.

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/3e637332-258c-4af7-bf06-76491e2422a1)

##### 2.2. Odak Uzaklığı Kamerada yüzeye yansıtılan görüntünün göreceli boyutu odak uzaklığına bağlıdır.
##### İğne deliği modelinde odak uzaklığı, delik ile görüntünün yansıtıldığı alan arası mesafedir.
##### Thales Teoremine göre;
##### x: Nesnenin görüntüsü(Eksi işareti, görüntünün ters çevrilmesinden gelir.)
##### X: Nesnenin boyutu
##### Z: Delikten nesneye olan mesafe
##### f: Odak uzaklığı
##### Mercek tam olarak ortalanmadığından merceğin yatay ve dikey yer değiştirmeleri için sırasıyla Cx ve Cy olmak üzere iki parametre belirlenir. Görüntü alanı dikdörtgen olduğu için X ve Y eksenlerindeki odak uzunlukları da farklıdır. Nesnenin yüzeydeki konumu için aşağıdaki formül kullanılır;

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/db5e8c6a-9977-4151-bfaa-9a502b901a40)

##### Gerçekte ekrana yansıtılan noktalar aşağıdaki gibi modellenebilir. M burada içsel matristir.

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/0b1c5168-7c75-49b2-b259-e7d60711b3c0)

### 3. OpenCV ile Kalibrasyon 

##### Sağlıklı bir şekilde uzaklığı tespit edebilmek için merceğin şeklinden ve kameranın geometrisinden kaynaklanan sorunlardan kurtulmamız gerekir. Bunun için OpenCV kütüphanesinin belirli işlevleri ile içsel parametreleri hesaplayarak kameramızı kalibre edebiliriz. Bunun için bir satranç tahtasının farklı açılardaki görünümlerini kullanabiliriz.

##### OpenCV2nin kalibrasyon için kendi fotoğraf çekme fonksiyonu kullanarak her iki kamerada satranç tahtasının köşeleri algılandığında, algılanan görüntünün pencereleri açılır. Görüntüler kaydedilip kalibrasyon için uygun olmayan bozulan vs. görüntüler silinebilir. En uygun resimler, köşeleri net olarak görülebildiği resimlerdir. Pikseller bu köşeler ile eşleşir. Bu görüntüler daha sonra ana Stereo Görüş kodumuzun içinde kalibrasyon bölümünde kullanılacaktır. Ne kadar çok uygun resim olursa eğitim o kadar sağlıklı olur ve kalibrasyon da o kadar başarılı olur. OpenCV kalibrasyon için en az her bir kamera için 10 resim önermektedir. Kamera kalibre edildikten sonra kameramız için M matrisini elde ederiz. cv2.findChesssboardCorners, fonksiyonu kameraları kalibre etmek için satranç tahtasının köşelerini bulur. Her görüntü için köşelerin konumu görüntü vektöründe saklanır. 3 boyutlu sahne için nesne noktaları başka bir vektörde saklanır. cv2.calibrateCamera işlevinde Imgpoints ve Objpoints kullanılır. Kamera matrisi, bozulma katsayıları, döndürme ve öteleme vektörlerini ifade eder. cv2.getOptimalNewCameraMatrix, daha sonra cv2.stereoRecify işlevinde kullanacağımız kesin kamera matrislerini ifade eder.

### 4. Stereo Görüntüleme

##### Stereo görüşte, görüntüdeki derinlik, ölçüm yapmayı ve 3 boyutlu yapılandırma işlemini ve konumlamayı sağlar. İki kamera arasında eşleşen noktalar bulunmalıdır.

#### 4.1. Üçgenleme 

##### Her iki projeksiyon görüntüsünün de eş düzlemli olduğunu ve sol görüntünün yatay piksel sırasının, karşılıklı gelen sol görüntünün yatay piksel sırası hizalandığını varsayar.

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/9fdebbf8-ec61-48b8-a8e3-b166c6f8d568)

##### P noktası karşıdaki nesnedir ve karşılıklı gelen xl ve xr koordinatlarıyla sol ve sağ görüntülerde pl ve pr ile eşlenir. P noktası ne kadar uzaktaysa d miktarının da o kadar küçük olduğu görülebilir. Yani aradaki fark uzaklıkla ters orantılıdır. Mesafe aşağıdaki formülle hesaplanabilir;

##### 𝑍 = 𝑓 ∗ 𝑇/(𝑥𝑙 − 𝑥𝑟)

##### Eşitsizlik sıfıra yakın olduğunda küçük eşitsizlik farkları büyük mesafe farklılıklarına neden olur. Eşitsizlik büyük olduğunda bu durum tersine döner. Stereo görüntünün yalnızca kameraya yakın nesneler için yüksek bir derinlik çözünürlüğüne sahip diyebiliriz. Eğer kameranın konfigürasyonu yeterli derecede iyiyse bu yöntem direk işe yarayabilir. Projemizde şartlar bu durum için yeterli değildir. Bu sebeple sağ ve sol kameraları matematiksel olarak aynı düzleme hizaladık.

#### 4.2. Epipolar Geometri

##### Epipolar geometri, farklı iki açıdan çekilmiş görüntüler arasındaki geometrik ilişkiyi açıklayan stereo görüş geometrisidir. Burada nesne farklı açılardan gözlemlenir. Eğer iki kameranın birbirlerinin arasındaki düzlemle yapılan açı biliniyorsa, P noktasının pozisyonu bulunabilir. P noktası, her iki kamerada bir miktar kaymış olarak görünür ancak bu kayma uzaktaki nesneler için daha az olur.

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/c1cc350d-c111-4e31-89e4-ccfa03e61acf)

##### İki kamera merkezi taban çizgisi tarafından birleşmektedir. Kamera çizgilerini birleştiren taban çizgisi ile görüntü düzlemerinin kesiştiği noktalar epipol(el,er) noktalardır. Epipolar çizgiler genellikle x eksenine paralel değillerdir. Bu nedenle görüntüler, epipolar çizgileri birbirine paralel olacak şekilde dönüştürülür. Bu işlem “görüntü normalizasyonu”dur. Aşağıda epipolar geomoetri ile normalize edilmiş görüntü ile normal görüntü farkı görülmektedir.

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/82290d51-6020-45f7-bc21-79a3203fba0e)

#### 4.3. Stereo Görüntünün İyileştirilmesi:

##### İki görüntüyü tam olarak aynı düzlemde olacak şekilde yansıtmak için epipolar çizgiler yatay olacak şekilde hizalanır. Bouguet algoritması kullanarak yaptığımız bu işlem sonucunda;
* Bozulma vektörü
* Görüntüye uygulanacak döndürme matrisi Rrect
* Düzeltilmiş bir kamera matrisi Mrect
* Düzeltilmemiş kamera matrisi M, elde edilir.
##### Bouguiet, her iki yansıtılan düzlemi de aynı düzlemde olacak şekilde yarım tur döndürmek için hesaplanan döndürme ve öteleme matrisini kullanır. Işınları paralel ve düzlemleri eş düzlemli yapar. WLS(The Weight Least Squares) Filtresi: Ağırlıklı en küçük kareler (WLS) filtresi, iyi bilinen bir kenar koruma yumuşatma tekniğidir, ancak ağırlıkları büyük ölçüde görüntü gradyanlarına bağlıdır. Hem izotropilerine hem de gradyanlarına dayalı olarak pikseller için yumuşatma ağırlıklarının hesaplanmasına yardımcı olur.

#### 5.1.KALİBRASYON İÇİN GÖRÜNTÜLERİN ELDE EDİLME DİYAGRAMI

##### Stereo Görüntüleme ile mesafe tespitinde ilk aşama, kameralardan kaynaklanan bozulmaları düzeltmek için satranç tahtası yardımı ile kalibrasyon yapmaktır. Kalibrasyon ayarı için kameralardan belli sayılarda görüntü alınması gerekir. Aşağıda kalibrasyon için alınan görüntülerin elde edilmesinin algoritma şeması bulunmaktadır:

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/a97580c7-0744-4ca9-bd55-458afe08ed26)

#### 5.2. STEREO GÖRÜŞ İLE MESAFE TESPİTİNİN YAPILMASI BLOK DİYAGRAMI

##### Ana kodda ise kameralar ilk önce bozulmayı gidermek için ayrı ayrı kalibre edilir. Ardından stereo kalibrasyon yapılır yani döndürme ortadan kaldırılır ve epipolar çizgiler hizalanır. Döngü içinde görüntüler işlenir ve eşitsizlik haritası oluşturulur.

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/8c595323-ea72-4e3c-a445-ebf0c8160db0)

##### Sonuç:

![image](https://github.com/ByteSparkYazilim/Cift_kamerali_mesafe_tespit_cihazi/assets/145047961/1e9dc922-520f-4e63-917a-ecea2f0d8524)

##### Bu çalışmanın yazılımları, python3.8 üzerinde hazırlanmış olup; Raspbian Buster, Ubuntu 20.04 LTS dağıtımlarında test edilmiştir. Çalışmanın yazılımları, veri gizliliği sebebiyle paylaşılmamıştır. Detaylı bilgi almak için iletişime geçiniz.










