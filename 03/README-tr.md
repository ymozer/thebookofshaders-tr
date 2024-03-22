## Uniform'lar

Şuana kadar GPU'nun paralel çok sayıda iş parçacıklarını nasıl yönettiğini ve herbirinin toplam görüntünün bir kısmına rengi atadığını gördük. Her bir paralel iş parçacığının birbirinine kör olmasına rağmen, CPU'dan tüm iş parçacıklarına bazı girdiler göndermemiz gerekiyor. Grafik kartının mimarisi gereği bu girdiler tüm iş parçacıklarına eşit (*uniform*) olacak ve zorunlu olarak *salt okunur* olarak ayarlanacak. Başka bir deyişle, her iş parçacığı aynı veriyi alacak ve okuyabilecek ancak değiştiremeyecek.

bu girdilere `uniform` denir ve desteklenen türlerin çoğunda gelir: `float`, `vec2`, `vec3`, `vec4`, `mat2`, `mat3`, `mat4`, `sampler2D` ve `samplerCube`. Uniformlar, varsayılan kayan nokta hassasiyetini atadıktan hemen sonra shader'ın en üstünde ilgili türle tanımlanır.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;  // Tuval boyutu (genişlik, yükseklik)
uniform vec2 u_mouse;       // Ekran pikseli cinsinden fare pozisyonu
uniform float u_time;       // Yüklenmeden bu yana geçen saniye
```

Uniform'ları CPU ve GPU arasındaki küçük köprüler gibi hayal edebilirsiniz. İsimler uygulamadan uygulamaya değişecektir ancak bu örnek serisinde her zaman: `u_time` (shader'ın başladığı andan itibaren geçen süre), `u_resolution` (shader'ın çizildiği tuval boyutu) ve `u_mouse` (piksel cinsinden tuval içindeki fare pozisyonu) gibi değişkenleri geçiriyorum. Bu değişkenin doğasını açıkça belirtmek için değişken adından önce `u_` eklemeyi tercih ediyorum ancak uniformlar için her türlü isim bulabilirsiniz. Örneğin [ShaderToy.com](https://www.shadertoy.com/) aynı uniformları kullanır ancak aşağıdaki isimlerle:

```glsl
uniform vec3 iResolution;   // Çizim alanı (tuval) çözünürlüğü (piksel cinsinden)    
uniform vec4 iMouse;        // Fare piksel koordinatları. xy: anlık, zw: tıklama
uniform float iTime;        // Shader oynatma süresi (saniye)
```

Yeterince konuştuk, şimdi uniformları eylemde görelim. Aşağıdaki kodda, `u_time` - shader'ın çalışmaya başladığı andan itibaren geçen saniye sayısı - ile bir sinüs fonksiyonunu kullanarak çizim alanındaki kırmızı miktarının geçişini animasyonlaştırıyoruz.

<div class="codeAndCanvas" data="time.frag"></div>

Gördüğünüz gibi GLSL daha fazla sürpriz sunuyor. GPU, açı, trigonometrik ve üstel fonksiyonları donanım hızlandırmalı olarak sunar. Bu fonksiyonlardan bazıları şunlardır: [`sin()`](../glossary/?search=sin), [`cos()`](../glossary/?search=cos), [`tan()`](../glossary/?search=tan), [`asin()`](../glossary/?search=asin), [`acos()`](../glossary/?search=acos), [`atan()`](../glossary/?search=atan), [`pow()`](../glossary/?search=pow), [`exp()`](../glossary/?search=exp), [`log()`](../glossary/?search=log), [`sqrt()`](../glossary/?search=sqrt), [`abs()`](../glossary/?search=abs), [`sign()`](../glossary/?search=sign), [`floor()`](../glossary/?search=floor), [`ceil()`](../glossary/?search=ceil), [`fract()`](../glossary/?search=fract), [`mod()`](../glossary/?search=mod), [`min()`](../glossary/?search=min), [`max()`](../glossary/?search=max) ve [`clamp()`](../glossary/?search=clamp).

Şimdi yukarıdaki kodla oynamanın zamanı geldi.

* Frekansı, renk değişimini neredeyse fark edilmeyecek kadar yavaşlatın.

* Tek bir rengi titreme olmadan görene kadara hızlandırın.

* İlginç desenler ve davranışlar elde etmek için farklı frekanslardaki üç kanalla (RGB)  oynayın.

## gl_FragCoord

GLSL bize varsayılan bir çıkış, `vec4 gl_FragColor` verdiği gibi, aynı zamanda varsayılan bir giriş, `vec4 gl_FragCoord` verir. Bu giriş, aktif iş parçacığın üzerinde çalıştığı *piksel* veya *ekran parçacığının* ekran koordinatlarını tutar. `vec4 gl_FragCoord` ile bir iş parçacığının nerede çalıştığını biliyoruz. Bu durumda bunu `uniform` olarak adlandırmıyoruz çünkü iş parçacıktan iş parçacığına farklı olacak, bunun yerine `gl_FragCoord` bir *varying* olarak adlandırılır.

<div class="codeAndCanvas" data="space.frag"></div>

Yukarıdaki kodda, piksel koordinatını, çizim alanının toplam çözünürlüğüne bölerek *normalize* ediyoruz. Bunu yaparak değerler `0.0` ve `1.0` arasında olacak ve X ve Y değerlerini KIRMIZI ve YEŞİL kanala eşlemek kolaylaşacak.

Shader dünyasında, hata ayıklama için çok fazla kaynağımız yoktur, değişkenlere güçlü renkler atamaktan başka bir şey yapamayız ve onları anlamaya çalışabiliriz. Bazı zamanlar GLSL'de kod yazmanın gemileri şişelerin içine koymak kadar zor, güzel ve tatmin edici olduğunu keşfedeceksiniz.

![](08.png)

Şimdi bu kodun anlamını sorgulamaya çalışma zamanı.


* `(0.0, 0.0)` koordinatının çizim alanımızda nerede olduğunu tahmin edebilir misiniz?

* Peki ya `(1.0, 0.0)`, `(0.0, 1.0)`, `(0.5, 0.5)` ve `(1.0, 1.0)` koordinatlarını?

* Değerlerin normalize DEĞİL de piksel cinsinden olduğunu bilerek, `u_mouse` değişkenini kullanarak renkleri nasıl hareket ettirebileceğinizi düşünebilir misiniz?

*  `u_time` ve `u_mouse` koordinatlarını kullanarak renk desenini değiştirmenin bir yolunu düşünebilir misiniz?

Bu egzersizleri yaptıktan sonra yeni shader-güçlerinizi nerede daha deneyebileceğinizi merak edebilirsiniz. Bir sonraki bölümde, kendi shader araçlarınızı nasıl yapacağınızı üç.js, Processing ve openFrameworks'te göreceğiz.
