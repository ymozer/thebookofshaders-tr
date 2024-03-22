Uniform değişkenlerini nasıl kullanacığınızı öğrenin. Uniform değişkenler, ya da basitçe *uniformlar* shader'ınızın tüm iş parçacıklarından eşit şekilde erişilebilen bilgileri taşıyan değişkenlerdir. [GSLS editörü](http://editor.thebookofshaders.com/) sizin için üç uniform değişkeni ayarlamıştır.

```glsl
uniform vec2 u_resolution; // Çizim alanı boyutu (genişlik, yükseklik)
uniform vec2 u_mouse;      // Ekran pozisyonu cinsinden fare pozisyonu
uniform float u_time;	  // Yüklenmeden bu yana geçen süre (saniye cinsinden)
```
