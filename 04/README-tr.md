## Shader'larınızı çalıştırma

Bu kitabın yapımı ve sanat pratiğim için, shader'ları oluşturmak, görüntülemek, paylaşmak ve sergilemek için bir ekosistem aracı oluşturdum. Bu araçlar, kod üstünde herhangi bir değişikliğe ihtiyaç duymadan Linux, MacOS, Windows ve [Raspberry Pi](https://www.raspberrypi.org/) ve tarayıcılarda tutarlı bir şekilde çalışır.

## Shader'larınızı tarayıcıda çalıştırma

**Görüntüleme**: bu kitaptaki tüm canlı örnekler, [glslCanvas](https://github.com/patriciogonzalezvivo/glslCanvas) kullanılarak görüntülenir. Bu, bağımsız shader'ları çalıştırma sürecini inanılmaz derecede kolaylaştırır.

```html
<canvas class="glslCanvas" data-fragment-url=“yourShader.frag" data-textures=“yourInputImage.png” width="500" height="500"></canvas>
```

Gördüğünüz gibi, sadece `class="glslCanvas"` ve `data-fragment-url`'deki shader'ınızın URL'si olan bir `canvas` öğesine ihtiyacınız var. Daha fazlasını öğrenmek için [buraya](https://github.com/patriciogonzalezvivo/glslCanvas) tıklayabilirsiniz.

Eğer benim gibiyseniz, muhtemelen shader'ları doğrudan konsoldan çalıştırmak isteyeceksiniz, bu durumda [glslViewer](https://github.com/patriciogonzalezvivo/glslViewer) uygulamasını kontrol etmelisiniz. Bu uygulama, shader'ları `bash` betiklerinizde veya unix pipe'larınızda kullanmanıza ve [ImageMagick](http://www.imagemagick.org/script/index.php) gibi bir şekilde kullanmanıza olanak tanır. Ayrıca [glslViewer](https://github.com/patriciogonzalezvivo/glslViewer) shader'ları [Raspberry Pi](https://www.raspberrypi.org/) üzerinde derlemek için de harika bir yoldur, bu yüzden [openFrame.io](http://openframe.io/) bunu shader sanatını göstermek için kullanır. Daha fazla bilgi için [buraya](https://github.com/patriciogonzalezvivo/glslViewer) tıklayabilirsiniz.

```bash
glslViewer shaderDosyanız.frag girdiFoto.png —w 500 -h 500 -E screenshot,çıktıFoto.png
```

**Oluşturma**: shader kodu yazma deneyimini aydınlatmak için [glslEditor](https://github.com/patriciogonzalezvivo/glslEditor) adında bir çevrimiçi düzenleyici yaptım. Bu düzenleyici, kitabın canlı örneklerinde gömülüdür ve glsl kodu ile çalışmanın soyut deneyimini daha somut hale getirmek için bir dizi kullanışlı widget getirir. Ayrıca [editor.thebookofshaders.com/](http://editor.thebookofshaders.com/) adresinden bağımsız bir web uygulaması olarak çalıştırabilirsiniz. Daha fazla bilgi için [buraya](https://github.com/patriciogonzalezvivo/glslEditor) tıklayabilirsiniz.

![](glslEditor-01.gif)

Eğer [SublimeText](https://www.sublimetext.com/) kullanarak çevrimdışı çalışmayı tercih ederseniz, [glslViewer için bu paketi](https://packagecontrol.io/packages/glslViewer) yükleyebilirsiniz. Daha fazla bilgi için [buraya](https://github.com/patriciogonzalezvivo/sublime-glslViewer).

![](glslViewer.gif)

**Paylaşma**: çevrimiçi düzenleyici olan ([editor.thebookofshaders.com/](http://editor.thebookofshaders.com/))'u kullanarak shader'larınızı paylaşabilirsiniz! Hem gömülü hem de bağımsız sürüm, shader'larınız için benzersiz URL'ler alabileceğiniz bir paylaş düğmesine sahiptir. Ayrıca [openFrame.io](http://openframe.io/) için doğrudan paylaş yapma yeteneğine sahiptir.

![](glslEditor-00.gif)

**Galeri**: Kodunuzu paylaşmak, shader'ınızı bir sanat eseri olarak paylaşmanın başlangıcıdır! [openFrame.io](http://openframe.io/)’a dışa aktarma seçeneğinin yanı sıra, shader'larınızı herhangi bir siteye gömülebilecek bir galeriye küratörlük yapmak için bir araç olan [glslGallery](https://github.com/patriciogonzalezvivo/glslGallery) adında bir araç yaptım. Daha fazla bilgi için [buraya](https://github.com/patriciogonzalezvivo/glslGallery) tıklayabilirsiniz.

![](glslGallery.gif)

## Favori framework'ünüzde shader'larınızı çalıştırma

Eğer zaten [Processing](https://processing.org/), [Three.js](http://threejs.org/), [OpenFrameworks](http://openframeworks.cc/) veya [SFML](https://www.sfml-dev.org/) gibi bir framework'te programlama deneyiminiz varsa, muhtemelen bu kitapta kullanacağımız aynı uniform'ları kullanarak bu platformlarda shader'ları denemek istersiniz. Aşağıda, bu kitapta bulacağınız shader'ları ayarlamak için bazı popüler framework'lerde nasıl yapılacağına dair örnekler bulunmaktadır. (Bu bölümün [GitHub deposunda](https://github.com/patriciogonzalezvivo/thebookofshaders/tree/master/04) bu dört framework için tam kaynak kodunu bulacaksınız.)

### **Three.js**

[MrDoob](https://twitter.com/mrdoob) olarak bilinen parlak ve çok mütevazi Ricardo Cabello, diğer [katkıda bulunanlar](https://github.com/mrdoob/three.js/graphs/contributors) ile birlikte muhtemelen en ünlü WebGL framework'lerinden birini geliştirdi, [Three.js](http://threejs.org/). Bu JavaScript kütüphanesini kullanarak harika 3D grafikler yapmayı öğreten birçok örnek, öğretici ve kitap bulacaksınız.

Aşağıda, Three.js'de shader'ları başlatmak için ihtiyacınız olan HTML ve JS'nin bir örneği bulunmaktadır. Shader'ları bu kitapta bulduğunuz yerden kopyalayabileceğiniz `id="fragmentShader"` betiğine dikkat edin.

```html
<body>
    <div id="container"></div>
    <script src="js/three.min.js"></script>
    <script id="vertexShader" type="x-shader/x-vertex">
        void main() {
            gl_Position = vec4( position, 1.0 );
        }
    </script>
    <script id="fragmentShader" type="x-shader/x-fragment">
        uniform vec2 u_resolution;
        uniform float u_time;

        void main() {
            vec2 st = gl_FragCoord.xy/u_resolution.xy;
            gl_FragColor=vec4(st.x,st.y,0.0,1.0);
        }
    </script>
    <script>
        var container;
        var camera, scene, renderer, clock;
        var uniforms;

        init();
        animate();

        function init() {
            container = document.getElementById( 'container' );

            camera = new THREE.Camera();
            camera.position.z = 1;

            scene = new THREE.Scene();
            clock = new THREE.Clock();

            var geometry = new THREE.PlaneBufferGeometry( 2, 2 );

            uniforms = {
                u_time: { type: "f", value: 1.0 },
                u_resolution: { type: "v2", value: new THREE.Vector2() },
                u_mouse: { type: "v2", value: new THREE.Vector2() }
            };

            var material = new THREE.ShaderMaterial( {
                uniforms: uniforms,
                vertexShader: document.getElementById( 'vertexShader' ).textContent,
                fragmentShader: document.getElementById( 'fragmentShader' ).textContent
            } );

            var mesh = new THREE.Mesh( geometry, material );
            scene.add( mesh );

            renderer = new THREE.WebGLRenderer();
            renderer.setPixelRatio( window.devicePixelRatio );

            container.appendChild( renderer.domElement );

            onWindowResize();
            window.addEventListener( 'resize', onWindowResize, false );

            document.onmousemove = function(e){
              uniforms.u_mouse.value.x = e.pageX
              uniforms.u_mouse.value.y = e.pageY
            }
        }

        function onWindowResize( event ) {
            renderer.setSize( window.innerWidth, window.innerHeight );
            uniforms.u_resolution.value.x = renderer.domElement.width;
            uniforms.u_resolution.value.y = renderer.domElement.height;
        }

        function animate() {
            requestAnimationFrame( animate );
            render();
        }

        function render() {
            uniforms.u_time.value += clock.getDelta();
            renderer.render( scene, camera );
        }
    </script>
</body>
```

### **Processing**

2001 yılında [Ben Fry](http://benfry.com/) ve [Casey Reas](http://reas.com/) tarafından başlatılan [Processing](https://processing.org/), kod üzerinde ilk adımlarınızı atmak için son derece basit ve güçlü bir ortamdır (en azından benim için öyleydi). [Andres Colubri](https://codeanticode.wordpress.com/), Processing'de OpenGL ve video üzerinde önemli güncellemeler yapmış ve bu dostane ortamda GLSL shader'larını kullanmak ve oynamak daha kolay hale getirmiştir. Processing, sketch'ın `data` klasöründe `"shader.frag"` adlı shader'ı arayacaktır. Bu yüzden burada bulduğunuz örnekleri kopyalayın ve dosyayı yeniden adlandırın.

```cpp
PShader shader;

void setup() {
  size(640, 360, P2D);
  noStroke();

  shader = loadShader("shader.frag");
}

void draw() {
  shader.set("u_resolution", float(width), float(height));
  shader.set("u_mouse", float(mouseX), float(mouseY));
  shader.set("u_time", millis() / 1000.0);
  shader(shader);
  rect(0,0,width,height);
}
```

Shader kodunuzun Processing'in 2.1 versiyonundan önceki versiyonlarda çalışması için, shader'ın başına şu satırı eklemeniz gerekmektedir: `#define PROCESSING_COLOR_SHADER`. Bu yüzden şu şekilde olmalıdır:

```glsl
#ifdef GL_ES
precision mediump float;
#endif

#define PROCESSING_COLOR_SHADER

uniform vec2 u_resolution;
uniform vec3 u_mouse;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.st/u_resolution;
    gl_FragColor = vec4(st.x,st.y,0.0,1.0);
}
```

Processing hakkında daha fazla bilgi için bu [öğreticiye](https://processing.org/tutorials/pshader/) göz atabilirsiniz.

### **openFrameworks**

Herkesin kendini rahat hissettiği bir yer vardır, benim durumumda bu yer hala [openFrameworks topluluğu](http://openframeworks.cc/)'dur. Bu C++ kütüphanesi (framework'ü), OpenGL ve diğer açık kaynak C++ kütüphaneleri etrafında sarılır. Birçok yönden Processing'e çok benzer, ancak C++ derleyicileriyle uğraşmanın açık komplikasyonları vardır. openFrameworks, Processing gibi, shader dosyalarınızı `data` klasöründe arayacaktır, bu yüzden kullanmak istediğiniz `.frag` dosyalarını kopyalayıp yüklediğinizde adını değiştirmeyi unutmayın.

```cpp
void ofApp::draw(){
    ofShader shader;
    shader.load("","shader.frag");

    shader.begin();
    shader.setUniform1f("u_time", ofGetElapsedTimef());
    shader.setUniform2f("u_resolution", ofGetWidth(), ofGetHeight());
    ofRect(0,0,ofGetWidth(), ofGetHeight());
    shader.end();
}
```

Eğer GlslViewer ve GlslCanvas'in kullanımında bulunan tüm uniform'ları daha basit bir şekilde OpenFrameworks'te kullanmak istiyorsanız, [ofxShader](https://github.com/patriciogonzalezvivo/ofxshader) eklentisini kullanmanızı öneririm. Bu eklenti ayrıca birden fazla buffer, materyal shader'ları, hotreload ve Raspberry Pi'de OpenGL ES için otomatik dönüşüm desteği sağlayacaktır. Ve kodunuz şu şekilde olacaktır:

```cpp
//--------------------------------------------------------------
void ofApp::setup(){
    ofDisableArbTex();
    
    sandbox.allocate(ofGetWidth(), ofGetHeight());
    sandbox.load("grayscott.frag");
}

//--------------------------------------------------------------
void ofApp::draw(){
    sandbox.render();
    sandbox.draw(0, 0);
}
```

openFrameworks içindeki shader'lar hakkında daha fazla bilgi için [Joshua Noble](http://thefactoryfactory.com/) tarafından hazırlanmış [bu harika öğreticiye](http://openframeworks.cc/ofBook/chapters/shaders.html) göz atabilirsiniz.