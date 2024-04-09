## Вступ для тих, хто прийшов із JS
автор [Nicolas Barradeau](http://www.barradeau.com/)


Якщо ви JavaScript-розробник, швидше за все, ви будете трохи спантеличені, читаючи книгу.
Дійсно, є багато відмінностей між маніпулюванням високорівневим JS і длубанням у менш високорівневих шейдерах.
Проте, на відміну від більш низькорівневої мови асемблера, GLSL є людино-зрозумілою мовою, і я впевнений, що як тільки ви розберетеся з її особливостями, то швидко запрацюєте.

Я припускаю, що ви маєте хоча б поверхневе знання про JavaScript і API Canvas.
Якщо ні, не хвилюйтеся, ви все одно зможете зрозуміти більшу частину цього розділу.

Крім того, я не буду надто вдаватися в подробиці, і деякі речі можуть бути _напівправдою_, тож не розраховувайте на "вичерпний посібник", а радше на ознайомлення.

### Введення для новачків

JavaScript чудово підходить для швидкого прототипування; ви накидуєте купу різноманітних нетипізованих змінних і методів, можете динамічно додавати та видаляти члени класів, оновлювати сторінку і швидко перевіряти чи вона працює.
Потім можна внести нові зміни, оновити сторінку, повторити - легке життя.
Тож ви можете поставити собі питання, у чому різниця між JavaScript і GLSL?
Зрештою, обидва працюють у браузері, обидва використовуються для малювання купи прикольних штук на екрані, і в цьому сенсі JS легше використовувати.

Що ж, головна відмінність полягає в тому, що Javascript є **інтерпретованою** мовою, тоді як GLSL є **компільованою**.
**Скомпільована** програма виконується нативно в ОС, є низькорівневою і загалом швидкою.
**Інтерпретована** програма потребує для свого виконання [віртуальну машину](https://en.wikipedia.org/wiki/Virtual_machine) (VM), є високорівневою і відносно більш повільною.


Коли браузер (_**VM** для JavaScript_) **виконує** або **інтерпретує** фрагмент JS-коду, він ще не має уявлення про те, яка змінна чим являється і яка функція що робить (за винятком **типізованих масивів**). 
Тож він не може нічого оптимізувати _наперед_.
Потрібен деякий час, щоб прочитати ваш код, щоб **вивести**, на основі використання, типи ваших змінних та методів і, коли це можливо, перетворити _дещо_ з вашого коду на код асемблера, який виконуватиметься набагато швидше.

Це повільний, кропіткий і шалено складний процес. Якщо вас цікавлять деталі, я рекомендую подивитися, як [працює рушій V8 у Chrome](https://developers.google.com/v8/).
Найгірше те, що кожен браузер оптимізує JS по-своєму, а процес _прихований_ від вас. В цьому плані ви безсилі.

**Компільована** програма не потребує інтерпретації. Операційна система запускає її і якщо програма валідна, вона виконується.
Це велика відмінність. Якщо ви забули крапку з комою в кінці рядка, ваш код вже не валідний та не скомпілюється, він взагалі не перетвориться на програму.

Це суворо, але це те, чим є **шейдер**: _програмою, що компілюється для виконання на GPU_.
Не бійтеся! **Компілятор** який перевіряє правильність вашого коду, стане вашим найкращим другом.
Приклади цієї книги та [редактор коду](http://editor.thebookofshaders.com/) дуже дружні до користувача.
Вони підкажуть вам де і чому не вдалося скомпілювати вашу програму.
Потім, після потрібних виправлень, коли шейдер буде готовий до компіляції, він миттєво зобразить результат своєї роботи. Це чудовий спосіб навчання, оскільки він дуже наочний і безпечний, бо насправді ви нічого не зможете зламати.

Останнє зауваження: **шейдер** складається з 2 програм: **вершинного шейдера** і **фрагментного шейдера**.
Якщо коротко, то **вершинний шейдер** це перша програма, що отримує на вхід *геометрію* та перетворює її на серію **пікселів** (або *фрагментів*).
Потім вона передає їх до **фрагментного шейдера** - другої програми, яка вирішить, яким кольором пофарбувати ці пікселі.
Дана книга здебільшого зосереджена на **фрагментних шейдерах**. У всіх прикладах геометрія є простим чотирикутником, який охоплює весь екран.

Готові?

Поїхали!

### Сильні типи
![перший результат пошуку для 'strong type' в Google Image на 20.05.2016](strong_type.jpg)

Коли ви приходите з JS або будь-якої іншої нетипізованої мови, **типізація** змінних являється для вас чужорідною концепцією, що стає найважчим кроком до GLSL.
**Типізація**, як випливає з назви, означає, що вам потрібно надавати **тип** усім своїм змінним і функціям.
Це фактично означає, що слів **`var`** або **`let`** більше не існує.
Поліція думок GLSL стерла їх із загальної мови й ви більше не можете їх використовувати, тому що, ну... їх не існує.

Замість використання чарівного слова **`var`** вам доведеться _явно вказати тип кожної змінної_, яку ви використовуєте, тоді компілятор бачитиме лише ті об'єкти та примітиви, з якими він вміє ефективно поводитися.
Мінус, що ви не можете використовувати ключове слово **`var`** і повинні _явно вказувати всі типи_, полягає в тому, що вам доведеться знати типи усіх змінних і знати їх добре.
Будьте спокійні, їх небагато і вони досить прості (GLSL це вам не Java-фреймворк).

Це може здатися страшним, але загалом це не дуже відрізняється від того, що ви робите на JavaScript. Наприклад, якщо ви маєте `boolean`-змінну, то ви очікуєте, що вона зберігатиме лише `true` або `false` і нічого більше.
Якщо змінна називається `var uid = XXX;`, то в ній вірогідно зберігається цілочисельне значення, а `var y = YYY;` _може_ бути посиланням на значення з рухомою крапкою.
Що ще краще, завдяки **сильним типам** ви не витрачатимете час на роздуми про те, чи `X == Y` (чи `typeof X == typeof Y`?, чи `typeof X !== null && Y...`). Ви просто *знаєте* це, а якщо ні, то компілятор знатиме напевно.

Ось **скалярні типи** (**скаляр** описує величину), які можна використовувати в GLSL: `bool` (булів тип), `int` (ціле число), `float` (число з рухомою крапкою).
Є й інші типи, але не будемо поспішати. Наступний фрагмент показує, як оголосити **`vars`** (так, я використав заборонене слово) у GLSL:
```glsl
// логічне значення
JS: var b = true;               GLSL: bool b = true;

// цілочисельне значення
JS: var i = 1;                  GLSL: int i = 1;

// числове значення з рухомою крапкою
JS: var f = 3.14159;            GLSL: float f = 3.14159;
```
Не дуже важко, правда? Як згадувалося вище, це навіть полегшує роботу, оскільки ви не витрачаєте час на перевірку типів даних змінних.
Якщо це здається сумнівним, то пам'ятайте, що ви робите це для того, щоб ваша програма виконувалася набагато швидше, ніж на JS.

#### void
Тип `void` приблизно відповідає `null`. Він використовується в якості типу, який має повертати метод, коли він нічого не повертає.
Ви не можете призначати його змінним.

#### boolean
Як вам відомо, логічні значення здебільшого використовуються в умовних перевірках: "`if (myBoolean == true) {...} else {...}`".
Якщо умовне розгалуження є звичайним підходом для CPU, то для [паралельної природи](http://thebookofshaders/01/?lan=ua) GLSL це твердження є менш правдивим.
Використання умовних галужень навіть не рекомендується у більшості випадків і у книзі показано кілька альтернативних методів для вирішення цього обмеження.

#### приведення типів
Як казав [Боромир](https://en.wikipedia.org/wiki/Boromir), "не можна просто взяти та скомбінувати типізовані примітиви". На відміну від JavaScript, GLSL не дозволить вам виконувати операції між змінними різних типів.

Наприклад, цей код:
```glsl
int     i = 2;
float   f = 3.14159;

// спроба помножити ціле число на значення з рухомою крапкою
float   r = i * f;
```
не спрацює, тому що ви намагаєтеся схрестити **_кота_** і **_жирафа_**.
Проблема вирішується за допомогою **приведення типу**, що _змусить компілятор повірити_, що *`i`* має тип `float` без фактичної зміни типу *`i`*.
```glsl
// приведення типу цілочисельної змінної 'i' до float
float   r = float(i) * f;
```

Це як вдягти **_кота_** у **шкіру _жирафа_** і працюватиме належним чином (змінна `r` зберігатиме результат множення `i` на `f`).

Можна **привести** будь-який зі згаданих вище типів до будь-якого іншого. Зауважте, що приведення `float` до `int` поводитиметься як `Math.floor()`, оскільки видалятиме значення після рухомої крапки.
Приведення `float` або `int` до `bool` поверне `true`, якщо змінна не дорівнює нулю.

#### конструктор
**Типи** змінних також є **конструкторами класів** для самих себе. Змінну `float` фактично можна розглядати як _`екземпляр`_ класу _`float`_.

Наступні оголошення рівнозначно валідні:

```glsl
int     i = 1;
int     i = int(1);
int     i = int(1.9995);
int     i = int(true);
```
Це може здатися не дуже схожим на `скалярні` типи та не дуже відрізняється від **приведення типів**, але у цьому з'явиться більше сенсу, коли дійдемо до розділу *перевантаження*.

Отже, ми познайомилися з трьома `примітивними типами`, без яких ви не зможете жити, але, звісно, GLSL має й інші.

### Вектори
![перший результат пошуку для 'vector villain' у Google Image на 20.05.2016](vector.jpg)

У Javascript, як і в GLSL, вам знадобляться складніші способи маніпуляції даними, ось де **`вектори`** стануть у пригоді.
Я припускаю, що вам вже доводилося писати клас `Point` на JavaScript, для утримання значень `x` і `y`, що виглядав якось так:
```glsl
// визначення класу:
var Point = function(x, y) {
    this.x = x || 0;
    this.y = y || 0;
}

// створення екземляру:
var p = new Point(100, 100);
```

Як ми щойно побачили, це ДУЖЕ неправильно на ДУЖЕ багатьох рівнях! По-перше, ключове слово **`var`**, далі жахливе **`this`**, потім знову **нетипізовні** значення `x` і `y`...
Ні, це не запрацює в шейдерленді.

Натомість GLSL надає вбудовані структури даних для їх групування, а саме:

* **`bvec2`**: 2D вектор для bool, **`bvec3`**: 3D  вектор для bool, **`bvec4`**: 4D  вектор для bool
* **`ivec2`**: 2D вектор для int, **`ivec3`**: 3D вектор для int, **`ivec4`**: 4D вектор для int
* **`vec2`**: 2D вектор для float, **`vec3`**: 3D вектор для float, **`vec4`**: 4D вектор для float

Ви відразу помітили, що для кожного примітиву є відповідний **векторний** тип.
З показаного вище, ви можете зробити висновок, що `bvec2` буде містити два значення типу `bool`, а `vec4` — чотири значення `float`.

Також вектори вводять таку річ як **вимірність** або **розмірність**. Це не означає, що 2D-вектор використовується при малюванні 2D-графіки, а 3D-вектор при малюванні 3D-сцен. Ні!
Що в такому разі буде представляти 4D-вектор? (ну насправді це називається тесерактом або гіперкубом)

**Розмірність** представляє кількість і тип **компонентів** або **змінних**, що зберігаються у **векторі**:
```glsl
// створюємо двовимірний булів вектор
bvec2 b2 = bvec2(true, false);

// створюємо тривимірний цілочисельний вектор
ivec3 i3 = ivec3(0, 0, 1);

// створюємо чотиривимірний вектор з рухомою комою
vec4 v4 = vec4(0.0, 1.0, 2.0, 1.0);
```
`b2` зберігає два різних булевих значення, `i3` - 3 різні цілочисельні значення, а `v4` - 4 різні значення з рухомою комою.

Але як звернутися до цих значень?
У випадку `скалярів` відповідь очевидна: для "`float f = 1.2;`", змінна `f` містить значення `1.2`.
З **векторами** це трохи інакше і доволі красиво.

#### аксесори - доступи до елементів
Існують різні способи доступу до значень
```glsl
// створимо чотиривимірний вектор типу float
vec4 v4 = vec4(0.0, 1.0, 2.0, 3.0);
```
отримати кожне з 4-х значень, можна наступним чином:
```glsl
float x = v4.x;     // x = 0.0
float y = v4.y;     // y = 1.0
float z = v4.z;     // z = 2.0
float w = v4.w;     // w = 3.0
```
Просто і легко. Наведені нижче приклади показують інші еквівалентні способи доступу до цих даних:
```glsl
float x =   v4.x    =   v4.r    =   v4.s    =   v4[0];     // x = 0.0
float y =   v4.y    =   v4.g    =   v4.t    =   v4[1];     // y = 1.0
float z =   v4.z    =   v4.b    =   v4.p    =   v4[2];     // z = 2.0
float w =   v4.w    =   v4.a    =   v4.q    =   v4[3];     // w = 3.0
```

Кмітливий читач міг помітити три речі:
* `X`, `Y`, `Z`, `W` зазвичай використовуються в 3D-програмах для представлення 3D-векторів
* `R`, `G`, `B`, `A` використовуються для кодування кольорів і альфа-каналу
* `[0]`, `[1]`, `[2]`, `[3]` означає, що ми можемо звертатися до значень через індекси

Тож залежно від того, чи працюєте ви з дво- чи тривимірними координатами, кольором з альфа-каналом чи без нього, або просто з деякими довільними значеннями, ви можете вибрати найбільш відповідний тип і розмірність **вектора**.
Зазвичай двовимірні координати та вектори (в геометричному сенсі) зберігаються як `vec2`, `vec3` або `vec4`, кольори як `vec3` або `vec4`, якщо вам потрібна непрозорість. Але, в цілому, обмежень на те як використовувати вектори немає.
Наприклад, якщо ви хочете зберегти лише одне логічне значення в `bvec4`, це можливо, але буде марною витратою пам'яті.

**Примітка**: у шейдері значення кольорів (`R`, `G`, `B`, `A`) нормалізовані, тобто варіюються в діапазоні від 0 до 1, а не від 0 до 0xFF, тому для них краще використовувати тип float `vec4`, ніж цілочисельний тип `ivec4`.

Вже маємо хороший початок, але рушаймо далі!

#### змішування

З вектора можна отримати більше одного значення за раз. Скажімо, із `vec4` вам потрібні лише значення `X` і `Y`. У JavaScript вам довелося б написати щось подібне:
```glsl
var needles = [0, 1]; // розміщення 'x' і 'y' в нашій структурі даних
var a = [0, 1, 2, 3]; // наша структура даних 'vec4'
var b = a.filter(function(val, i, array) {
  return needles.indexOf(array.indexOf(val)) != -1;
});
// b = [0, 1]

// або більш буквально:
var needles = [0, 1];
var a = [0, 1, 2, 3]; // структура даних 'vec4'
var b = [a[needles[0]], a[needles[1]]]; // b = [0, 1]
```
Виглядає потворно. У GLSL ви можете отримати ці дані так:
```glsl
// створюємо 4D-вектор типу float
vec4 v4 = vec4(0.0, 1.0, 2.0, 3.0);

// і одночасно отримуємо лише X та Y
vec2 xy = v4.xy; // xy = vec2(0.0, 1.0);
```
Що це щойно сталося?! Коли ви **об'єднуєте аксесори**, GLSL елегантно повертає підмножину значень, які ви запросили, у найбільш відповідному **векторному** форматі.
Тож тут вектор — це структура даних із **довільним доступом**, схожий на масив як у JavaScript.
Таким чином, ви можете не тільки отримати підмножину ваших даних, але і вказати їх **порядок**, у якому вони вам потрібні. Наступний приклад змінює порядок отримання компонентів вектора:
```glsl
// створюємо чотиривимірний вектор типу float: R,G,B,A
vec4 color = vec4(0.2, 0.8, 0.0, 1.0);

// отримуємо компоненти кольору в порядку A,B,G,R
vec4 backwards = color.abgr; // backwards = vec4(1.0, 0.0, 0.8, 0.2);
```
І, звичайно, ви можете повернути той самий компонент кілька разів:
```glsl
// створюємо чотиривимірний вектор типу float: R,G,B,A
vec4 color = vec4(0.2, 0.8, 0.0, 1.0);

// отримуємо vec3 з компонентами GAG на основі каналів G і A з початкового кольору
vec3 GAG = color.gag; // GAG = vec4(0.8, 1.0, 0.8);
```

Це надзвичайно зручно для об'єднання частин вектору, виділення лише rgb-каналів із кольору RGBA тощо.


#### перевантаження

У розділі типів я згадував про **конструктор** і можливість **перевантаження**.
Для тих, хто не знає, **перевантаження** оператора або функції означає _'зміну поведінки зазначеного оператора або функції залежно від операндів/аргументів'_.
В JavaScript немає перевантаження, тому спочатку воно може здатися трохи дивним, але я впевнений, що як тільки ви звикнете до нього, то будете дивуватися, чому це не реалізовано в JS (коротка відповідь - *типізація*).

Найпростіший приклад перевантаженого оператора виглядає так:

```glsl
vec2 a = vec2(1.0, 1.0);
vec2 b = vec2(1.0, 1.0);
// перевантажене додавання
vec2 c = a + b;     // c = vec2(2.0, 2.0);
```
ЩО? Отже, можна додавати сутності, які не є числами?!

Саме так. Також це стосується всіх операторів (`+`, `-`, `*` і `/`), але це тільки початок.
Розглянемо наступний фрагмент:
```glsl
vec2 a = vec2(0.0, 0.0);
vec2 b = vec2(1.0, 1.0);
// перевантажений конструктор
vec4 c = vec4(a, b);         // c = vec4(0.0, 0.0, 1.0, 1.0);
```
Ми створили `vec4` з двох `vec2`. Таким чином новий `vec4` використав `a.x` і `a.y` як `X`і `Y` компоненти та `b.x` і `b.y` як `Z` і `W` компоненти для вектора `c`.

Ось що відбувається, коли **функція** перевантажується для прийняття різних аргументів, у попередньому випадку це був **конструктор** `vec4`.
Це означає, що в одній програмі може співіснувати багато **версій** одного і того самого методу з різною сигнатурою. Наприклад, усі наступні оголошення є валідними:
```glsl
vec4 a = vec4(1.0, 1.0, 1.0, 1.0);
vec4 a = vec4(1.0); // x, y, z, w - усі дорівнюють 1.0
vec4 a = vec4(v2, float, v4); // vec4(v2.x, v2.y, float, v4.x);
vec4 a = vec4(v3, float); // vec4(v3.x, v3.y, v3.z, float);
тощо
```
Єдине про що ви повинні попіклуватися, так це про передачу достатньої кількості аргументів для заповнення **вектору**.

Нарешті, ви також можете перевантажувати вбудовані функції у вашій програмі, щоб вони могли приймати аргументи, для яких не були розроблені (хоча це не повинно траплятися занадто часто).

#### більше типів
Вектори прикольні і є основою вашого шейдера.
А ще існують інші примітиви, такі як матриці та текстурні семплери, які будуть розглянуті пізніше в книзі.

Ми також можемо використовувати масиви. Звичайно, їх потрібно типізувати й вони мають свої особливості у порівнянні з JS:
* мають фіксований розмір
* ви не можете використовувати методи push(), pop(), splice() тощо, і немає властивості ```length```
* ви не можете відразу ініціалізувати їх значеннями
* Ви повинні встановити значення індивідуально

Це не спрацює:
```glsl
int values[3] = [0, 0, 0];
```
А ось це спрацює:
```glsl
int values[3];
values[0] = 0;
values[1] = 0;
values[2] = 0;
```
Добре, коли ви знаєте свої дані або маєте невеликі масиви значень.
Якщо вам потрібно більше виразності, то можете скористатися типом ```struct```. Це як _об'єкти_ без методів.
Вони дозволяють зберігати та отримувати доступ до кількох змінних всередині одного об'єкта:
```glsl
struct ColorStruct {
    vec3 color0;
    vec3 color1;
    vec3 color2;
}
```
Ви можете створити та отримати значення _colors_ наступним чином:
```glsl
// ініціалізуємо структуру деякими значеннями
ColorStruct sandy = ColorStruct(
 	vec3(0.92, 0.83, 0.60),
    vec3(1., 0.94, 0.69),
    vec3(0.95, 0.86, 0.69)
);

// отримуємо доступ до значень із структури
sandy.color0 // vec3(0.92, 0.83, 0.60)
```
Це синтаксичний цукор, але він може допомогти написати чистіший код, принаймні більш звичний для вас.

#### вирази та умови

Структури даних корисні, але нам _може_ знадобитися можливість для повторення дії або виконання умовних перевірок.
На щастя для нас, синтаксис дуже близький до JavaScript.
Умова виглядає так:
```glsl
if (condition) {
    // true
} else {
    // false
}
```
Звичайни цикл `for`:
```glsl
const int count = 10;
for (int i = 0; i <= count; i++) {
    // do something
}
```
Приклад циклу з ітератором типу float:
```glsl
const float count = 10.;
for (float i = 0.0; i <= count; i += 1.0) {
    // do something
}
```
Зауважте, що ```count``` потрібно визначити як ```константу```.
Це означає перед змінною потрібно додати **кваліфікатор** ```const```, який ми розглянемо трохи згодом.

Нам також доступні оператори ```break``` і ```continue```:
```glsl
const float count = 10.;
for (float i = 0.0; i <= count; i += 1.0) {
    if (i < 5.) continue;
    if (i >= 8.) break;
}
```
Зауважте, що на деяких типах пристроїв ```break``` не працює належним чином і завчасно не перериває виконання циклу.

Загалом, кількість ітерацій має бути якомога меншою, та і в цілому бажано уникати використання циклів і умовних галужень.


#### кваліфікатори

Окрім типів змінних, GLSL використовує **кваліфікатори**.
Коротко кажучи, кваліфікатори допомагають повідомити компілятору призначення змінних.
Наприклад, деякі дані для GPU можуть бути надані тільки від CPU і називаються **атрибутами** та **уніформами**.
**Атрибути** використовуються у вершинних шейдерах, а **уніформи** можна використовувати як у вершинних, так і у фрагментних шейдерах.
Існує також кваліфікатор ```variying```, який використовується для передачі змінних від вершинного шейдеру до фрагментного.

Я не буду вдаватися в деталі, оскільки ми зосереджені на **фрагментних шейдерах**, але далі в книзі ви побачите щось на кшталт:
```glsl
uniform vec2 u_resolution;
```
Бачите, що ми тут зробили? Ми додали кваліфікатор ```uniform``` перед типом змінної.
Це означає, що змінна, яка відповідає за роздільну здатність полотна з яким ми працюємо, передається шейдеру з CPU.
Ширина полотна зберігається в x, а висота в y-компоненті даного 2D-вектора.

Коли компілятор бачить змінну, якій передує цей кваліфікатор, він простежить, щоб ви не могли *змінити* такі значення під час рантайму.

Те саме стосується нашої змінної ```count```, яка слугувала обмеженням для циклу ```for```:
```glsl
const float count = 10.;
for ( ... )
```
Коли ми використовуємо кваліфікатор ```const```, компілятор не дає змогу перезаписувати значення такої змінної, інакше вона не була б константою.

У сигнатурах функцій можуть використовуватися 3 додаткові кваліфікатори: ```in```, ```out``` та ```inout```.
У JavaScript передані до функції значення скалярних аргументів доступні лише для читання. Якщо ви змінюєте їхні значення всередині функції, то ці зміни не застосовуються до змінної поза функцією.
```glsl
function banana(a) {
    a += 1;
}

var value = 0;
banana(value);
console.log(value); // 0 - значення за межами функції не змінилося
```

За допомогою кваліфікаторів перед аргументами ви можете вказати їх поведінку:
* ```in``` - лише для читання (за замовчуванням)
* ```out``` - лише для запису: можна змінити, але не можна прочитати значення
* ```inout``` - читання і запис: можна і прочитати й встановити нове значення

Переписаний метод banana у GLSL виглядає так:
```glsl
void banana(inout float a) {
    a += 1.;
}

float A = 0.;
banana(A); // тепер A = 1.;
```
Це дуже відрізняється від JS і є досить потужною можливістю, але вам не обов'язково вказувати кваліфікатори аргументів. За замовчуванням вони доступні лише для зчитування.

#### простір і координати

Останнє зауваження: у DOM і Canvas 2D ми звикли, що вісь Y спрямована 'вниз'.
Це має сенс у контексті DOM, оскільки відповідає способу розгортання вебсторінки: панель навігації вгорі, а контент прокручується донизу.
У полотні WebGL вісь Y перевернута і вказує 'вгору'.

Це означає, що початок координат, точка (0, 0), знаходиться в нижньому лівому куті контексту WebGL, а не у верхньому лівому куті, як у 2D Canvas.
Координати текстур також дотримуються цього правила, що спочатку може бути контрінтуїтивним.

## Ось і все!
Звісно, ми б могли більше заглибитися в різноманітні концепції, але, як згадувалося раніше, цей розділ написаний як просте введення для новачків.
Тут вже написано достатньо для того, щоб за деякий час переварити нові знання, але з терпінням і практикою ця мова ставатиме для вас все більш природною.

Сподіваюся, цей матеріал був корисним для вас. А тепер як щодо початку вашої подорожі основною частиною книги?