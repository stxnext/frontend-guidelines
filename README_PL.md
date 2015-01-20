# Dobre Praktyki JavaScriptu bazujace na Airbnb() {

*Najbardziej sensowne podejście do programowania w JavaScript'cie*


## Spis Treści

  1. [Typy](#typy)
  1. [Obiekty](#obiekty)
  1. [Tablice](#tablice)
  1. [Stringi](#stringi)
  1. [Funkcje](#funkcje)
  1. [Własności](#wlasnosci)
  1. [Zmienne](#zmienne)
  1. [Hoisting](#hoisting)
  1. [Warunki i równości](#warunki-i-rownosci)
  1. [Bloki kodu](#bloki-kodu)
  1. [Komentarze](#komentarze)
  1. [Białe znaki](#biale-znaki)
  1. [Przecinki](#przecinki)
  1. [Średniki](#sredniki)
  1. [Rzutowania i korekcje typów](#rzutowania-i-korekcje-typow)
  1. [Nazwy zmiennych i funkcji](#nazwy-zmiennych-i-funkcji)
  1. [Gettery i settery](#gettery-i-settery)
  1. [Konstruktory](#konstruktory)
  1. [Eventy](#eventy)
  1. [Moduły](#moduly)
  1. [jQuery](#jquery)
  1. [Standard ECMAScript 5](#standard-ecmascript-5)
  1. [Testowanie](#testowanie)
  1. [Wydajność](#wydajnosc)
  1. [Więcej do czytania](#wiecej-do-czytania)
  1. [Kto właściwie z tego korzysta w praktyce](#kto-wlasciwie-z-tego-korzysta-w-praktyce)
  1. [Tłumaczenia](#tlumaczenia)
  1. [Poradnik do poradnika tego poradnika](#poradnik-do-poradnika-tego-poradnika)
  1. [Pogadaj z nami o JavaScript'cie](#pogadaj-z-nami-o-javascriptcie)
  1. [Współtwórcy](#wspoltworcy)
  1. [Licencja](#licencja)

## Typy

  - **Typy prymitywne**: Kiedy operujesz na prymitywach, działasz bezpośrednio na ich wartościach. Zmienne prymitywne to:

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Typy złożone**: Pracując ze zmienną o złożonym typie, modyfikujesz wartość schowaną za referencją/wskaźnikiem. Zmienne zlożone to:

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;    // referencja do 'foo' przypisana do 'bar'

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ do góry](#table-of-contents)**

## Obiekty

  - Zamiast konstruktorów i `new`, używaj klamerek do tworzenia nowych obiektów.

    ```javascript
    // źle
    var item = new Object();

    // dobrze
    var item = {};
    ```

  - Nie używaj [słów kluczowych JavaScriptu](http://es5.github.io/#x7.6.1) jako nazw pól w tablicach i obiektach. Nie będą działać pod IE8. [Więcej info](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // źle
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // dobrze
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Zamiast słów kluczowych używaj sensownych synonimów.

    ```javascript
    // źle
    var superman = {
      class: 'alien'
    };

    // źle
    var superman = {
      klass: 'alien'
    };

    // dobrze
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ do góry](#table-of-contents)**

## Tablice

  - Używaj nawiasów kwadratowych do tworzenia tabic, zamiast konstruktorów.

    ```javascript
    // źle
    var items = new Array();

    // dobrze
    var items = [];
    ```

  - Jeżeli nie znasz długości tablicy, dodawaj do niej elementy przy pomocy metody Array#push.

    ```javascript
    var someStack = [];


    // źle
    someStack[someStack.length] = 'abracadabra';

    // dobrze
    someStack.push('abracadabra');
    ```

  - Do kopiowania tablic używaj metody Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // źle
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // dobrze
    itemsCopy = items.slice();
    ```

  - Aby zamienić tablico-podobny obiekt w tablicę, używaj metody Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ do góry](#table-of-contents)**


## Stringi

  - Używaj pojedynczego cudysłowu podczas zapisywania String'ów.

    ```javascript
    // źle
    var name = "Bob Parr";

    // dobrze
    var name = 'Bob Parr';

    // źle
    var fullName = "Bob " + this.lastName;

    // dobrze
    var fullName = 'Bob ' + this.lastName;
    ```

  - Tekst dłuższy niż 80 znaków powinieneś zapisywać dzieląc go na kilka osobnych linii.
  - Jedna uwaga: Przy nadmiernym stosowaniu powyższej zasady, łączone String'i mogą wpływać negatywnie na działanie aplikacji. [jsPerf](http://jsperf.com/ya-string-concat) & [Dyskusja na GitHub](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // źle
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // źle
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // dobrze
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - Kiedy tworzysz w kodzie długi tekst z kilku mniejszych, używaj metody Array#join zamiast dodawać Stringi przy pomocy operatora. Głównie ze względu na działanie pod IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items,
        messages,
        length,
        i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // źle
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // dobrze
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

**[⬆ na góre](#table-of-contents)**


## Funkcje

  - Sposoby zapisu funkcji w JavaScript'cie:

    ```javascript
    // funkcja anonimowa
    var anonymous = function() {
      return true;
    };

    // wyrażenie funkcji przy pomocy zmiennej
    var named = function named() {
      return true;
    };

    // funkcja wywołana natychmiast po utworzeniu (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Nigdy nie deklaruj nowej funkcji w bloku warunkowym ani iteracyjnym (if, while itp).  Przeglądarki pozwolą Ci na takie deklarowanie, ale każda będzie interpretowała to na swój sposób. Zamiast tego zapisuj funkcje przy pomocy zmiennej.
  - **Note:** ECMA-262 definiuje blok jako listę komend. Deklaracja funkcji to nie komenda. [Więcej o ECMA-262's i tym problemie](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // źle
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // dobrze
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Nie nadawaj żadnej funkcji argumentu o nazwie 'arguments'. Jest to jeden z domyślnie tworzonych parametrów dla każdej nowej funkcji i nie powinieneś go nadpisywać, zmieniać jego zachowania ani wartości.

    ```javascript
    // źle
    function nope(name, options, arguments) {
      // ...kod...
    }

    // dobrze
    function yup(name, options, args) {
      // ...kod...
    }
    ```


  - Pamiętaj, że źle wykorzystane domknięcia mogą prowadzić do wycieków pamięci.

    ```javascript
    function foo(element, a, b) {
      element.onclick = function() { /* uses a and b */ };
    }
    ```
  - W powyższym przykładzie event handler przypisany do zdarzenia onclick posiada referencję do wewnętrznego scope funkcji foo. Oznacza to że ma dostep do element, a i b, nawet jeżeli onclick nigdy nie zostanie wywołany. Tak długo jak event handler będzie przypisany do zdarzenia, tak długo garbage collector nie będzie mógł zwolnić pamięci, przeznaczonej dla a i b. W ten sposób powstają wycieki pamięci.

  - W takiej sytuacji kod powinien wyglądać następująco:

    ```javascript
    function foo(element, a, b) {
      element.onclick = bar(a, b);
    }

    function bar(a, b) {
      return function() { /* uses a and b */ };
    }
    ```
  - Więcej informacji o domknięciach: [closures](http://jibbering.com/faq/notes/closures/)

**[⬆ do góry](#table-of-contents)**



## <a name='wlasnosci'></a> Własności

  - Jeżeli nazwa parametru obiektu jest statyczna i znasz ją, używaj kropki, aby odczytać wartość tego parametru.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // źle
    var isJedi = luke['jedi'];

    // dobrze
    var isJedi = luke.jedi;
    ```

  - Jeżeli natomiast nazwa jest dynamiczna, używaj nawiasów kwadratowych.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ do góry](#table-of-contents)**


## Zmienne

  - Zawsze używaj słowa kluczowego 'var', aby tworzyć nowe zmienne. W przeciwnym wypadku będą one automatycznie przypisywane do bloku globalnego i mogą namieszać w aplikacji. Unikaj zmiennych globalnych zawsze, gdy jest to możliwe.

    ```javascript
    // źle
    superPower = new SuperPower();

    // dobrze
    var superPower = new SuperPower();
    ```

  - Jeżeli tworzysz kilka zmiennych, używaj do tego pojedynczego słówka 'var', a kolejne zmienne dodawaj w nowych linijkach, oddzielając jest przecinkami.

    ```javascript
    // źle
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // dobrze
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    ```

  - Niezainicjalizowane zmienne deklaruj zawsze na początku.

    ```javascript
    // źle
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // źle
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // dobrze
    var i;
    var length;
    var dragonball;

    var items = getItems();
    var goSportsTeam = true;
    ```

  - Nowe zmienne deklaruj zawsze na początku bloku w którym je tworzysz ( na początku scope'a ). JavaScript w przeciwnym razie przeniesie je tam za Ciebie 'w tle', a to może doprowadzić do niespodziewanych błędów ( Więcej w części - Hoisting ).

    ```javascript
    // źle
    function() {
      test();
      console.log('doing stuff..');

      //..kod..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // dobrze
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..kod..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // źle
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // dobrze
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ do góry](#table-of-contents)**


## Hoisting

  - Deklaracje zmiennych są automatycznie przenoszone przez JavaScript na początek bloku (w JS bloki tworzone są w obrębie funkcji) w którym zostały utworzone i w którym działają - tzw. scope'a. Przypisywane wartości nie są przenoszone. Dla osób, które zwykle używały języków C++/Java może się to wydać z początku lekko nielogiczne, ponieważ JavaScript pozwala na użycie zmiennej, która w danym miejscu w kodzie jeszcze nie istnieje.

    ```javascript
    // to nie powinno zadziałać (o ile tylko
    // nie ma zmiennej globalnej notDefined)
    function example() {
      console.log(notDefined); // => wyrzuca błąd ReferenceError
    }

    // deklaracja zmiennej, nawet po jej wykorzystaniu
    // w kodzie, pozbywa się błędu. Dzięki hoistingowi
    // deklaracja jest automatycznie przenoszona na
    // początek funkcji.
    // Wartość 'true' nie zostaje przeniesiona.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // poprzedni przykład, w rozumieniu interpretera
    // JavaScript'u, wygląda tak:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonimowe funkcje zapisane w zmiennych działają na podobnej zasadzie. Ich nazwa zostaje
    przeniesiona na początek scope'a, a sama wartość, tj. wnętrze funkcji, pozostaje na swoim
    miejscu.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Funkcje zapisane w zmiennych, posiadające swoją własną nazwę, tracą ją. Na początek scope'a
    wyniesiona zostaje nazwa zmiennej do której funkcja została przypisana.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // to samo dotyczy funkcji, których
    // nazwa jest taka sama jak nazwa
    // okupowanej zmiennej - ale to
    // w miarę logiczne
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Funkcje nieprzypisane do zmiennych zostają automatycznie wyniesione na początek scope'a.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - Więcej informacji na temat działania JavaScriptu, scope'ów i hoistingu: [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

**[⬆ na górę](#table-of-contents)**



## <a name='warunki-i-rownosci'></a> Warunki i równości

  - Używaj `===` i `!==` zamiast `==` i `!=`.
  - Wyrażenia warunkowe zawsze rzutowane są przy pomocy metody toBoolean(), której zasada działania wyglądaja następująco:

    + **Jakikolwiek Obiekt** równy jest **true**
    + **Undefined** równe jest **false**
    + **Null** równe jest **false**
    + **Boolean** równy jest **swojej wartości**
    + **Liczby** -  **false** jeżeli **+0, -0, lub NaN**, w przeciwnym razie **true**
    + **Stringi** równe są **false** jeżeli są puste `''`, w przeciwnym razie **true**

    ```javascript
    if ([0]) {
      // true
      // Tablica to obiekt, obiekty są zawsze równe true
    }
    ```

  - Staraj się zapisywać warunki w jak najkrótszej formie, jeżeli jest to możliwe.

    ```javascript
    // źle
    if (name !== '') {
      // ...kod...
    }

    // dobrze
    if (name) {
      // ...kod...
    }

    // źle
    if (collection.length > 0) {
      // ...kod...
    }

    // dobrze
    if (collection.length) {
      // ...kod...
    }
    ```

  - Więcej informacji: [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

**[⬆ na górę](#table-of-contents)**


## Bloki kodu

  - Jeżeli twój kod ma więcej niż jedną linijkę, obejmij go klamerkami.

    ```javascript
    // źle
    if (test)
      return false;

    // dobrze
    if (test) return false;

    // źle
    if (test) {
      return false;
    }

    // źle
    function() { return false; }

    // dobrze
    function() {
      return false;
    }
    ```

  - Nigdy nie używaj with.
  - Używanie with jest zabronione w ECMAScript 5 strict mode. W tym wypadku zaleca się zapisanie obiektu, którego wartość chcemy odczytać, do tymczasowej zmiennej.

    ```javascript
    // źle
    with(foo) {
      // sth
    }

    // dobrze
    var o = object;
    console.log(o.foo);
    ```

**[⬆ do góry](#table-of-contents)**


## Komentarze

  - Do długich, kilkuwierszowych komentarzy używaj zapisu `/** ... */`. W komentarzach zawieraj takie informacje jak: krótki opis, listę użytych typów zmiennych, argumenty funkcji i zwracane wartości.
kod
    ```javascript
    // źle
    // make() zwraca nowy element
    // na podstawie podanego argumentu
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...kod...

      return element;
    }

    // dobrze
    /**
     * make() zwraca nowy element
     * na podstawie podanego argumentu
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...kod...

      return element;
    }
    ```

  - Zapisu `//` używaj do krótkich, jednolinijkowych komentarzy. Komentarze umieszczaj linijkę ponad opisywanymi elementami. Nad komentarzami dodawaj też jedną pustą linijkę, dla czytelności.

    ```javascript
    // źle
    var active = true;  // jest aktywny

    // dobrze
    // jest aktywny
    var active = true;

    // źle
    function getType() {
      console.log('fetching type...');
      // ustaw domyslny typ na 'brak typu'
      var type = this._type || 'brak typu';

      return type;
    }

    // dobrze
    function getType() {
      console.log('fetching type...');

      // ustaw domyslny typ na 'brak typu'
      var type = this._type || 'brak typu';

      return type;
    }
    ```
  - Aby oznaczyć błędy, które trzeba będzie w przyszłości poprawić, używaj prefiksu 'FIXME'. Do oznaczania pomysłów, które mogą poczekać na swoją kolej, używaj 'TODO'. Pomoże to innym programistom w odszukaniu i zrozumieniu Twoich komentarzy. Takie specjalne znaczniki działają inaczej niż zwykłe komentarze. Nie niosą ze sobą konkretnych informacji, a raczej stanowią ogłoszenia dla innych programistów pracujących nad projektem. Przykładowo 'FIXME -- trzeba to później rozkminić' lub 'TODO -- trzeba by tutaj to dodać'.

  - Używaj `// FIXME:`, aby oznaczyć problemy w kodzie

    ```javascript
    function Calculator() {

      // FIXME: zmienna 'total' jest globalna
      total = 0;

      return this;
    }
    ```

  - Używaj `// TODO:`, aby oznaczyć pomysły

    ```javascript
    function Calculator() {

      // TODO: zmienna 'total' musi być konfigurowalna przez jakiś zewn. parametr
      this.total = 0;

      return this;
    }
  ```

**[⬆ do góry](#table-of-contents)**


## <a name='biale-znaki'></a> Białe znaki

  - Używaj tabulatorów o szerokości 4 spacji.

    ```javascript
    // źle
    function() {
    ∙∙var name;
    }

    // źle
    function() {
    ∙var name;
    }

    // dobrze
    function() {
    ∙∙∙∙var name;
    }
    ```

  - Dodawaj pojedynczą spację przed rozpoczynającymi blok klamrami.

    ```javascript
    // źle
    function test(){
        console.log('test');
    }

    // dobrze
    function test() {
        console.log('test');
    }

    // źle
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // dobrze
    dog.set('attr', {
        age: '1 year',
        breed: 'Bernese Mountain Dog'
    });
    ```

  - Rozdzielaj równania spacjami tak, aby były czytelne.

    ```javascript
    // źle
    var x=y+5;

    // dobrze
    var x = y + 5;
    ```

  - Na końcu plików dodawaj pojedynczy enter.

    ```javascript
    // źle
    (function(global) {
      // ...kod...
    })(this);
    ```

    ```javascript
    // źle
    (function(global) {
      // ...kod...
    })(this);↵
    ↵
    ```

    ```javascript
    // dobrze
    (function(global) {
      // ...kod...
    })(this);↵
    ```

  - Chain'ując metody wywołane na jednym elemencie, rozdzielaj je przy pomocy nowych linii i spacji.

    ```javascript
    // źle
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // dobrze
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // źle
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // dobrze
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

**[⬆ do góry](#table-of-contents)**

## Przecinki

  - **Nie** dodawaj przecinków przed zmiennymi w tablicach, obiektach i grupach.

    ```javascript
    // źle
    var once
      , upon
      , aTime;

    // dobrze
    var once,
        upon,
        aTime;

    // źle
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // dobrze
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - **Nie** dodawaj przecinka po ostatnim elemencie w tablicy. Może to wywołać błędy w starszych wersjach IE. W niektórych implementacjach ES3 powiększy to również tablicę o jeden niezidentyfikowany element - ten błąd nie dotyczy ES5 ([źródło](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // źle
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // dobrze
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ do góry](#table-of-contents)**


## <a name='sredniki'></a> Średniki

  - **Tak.**

    ```javascript
    // źle
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // dobrze
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // dobrze (taki zapis zapewnia poprawne działanie funkcji przed i po minimalizacji kodu przy pomocy np. Grunt'a, concat'a itp.)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Więcej na ten temat](http://stackoverflow.com/a/7365214/1712802).

**[⬆ do góry](#table-of-contents)**


## <a name='rzutowania-i-korekcje-typow'></a> Rzutowania i korekcje typów

  - Jeżeli to konieczne, dokonuj korekcji typu na początku wyrażenia.
  - Rzutowanie Stringów:

    ```javascript
    //  => this.reviewScore = 9;

    // źle
    var totalScore = this.reviewScore + '';

    // dobrze
    var totalScore = '' + this.reviewScore;

    // źle
    var totalScore = '' + this.reviewScore + ' total score';

    // dobrze
    var totalScore = this.reviewScore + ' total score';
    ```

  - Rzutowanie liczb. Używaj `parseInt` do zmiennych numerycznych i zawsze podawaj podstawę systemu liczbowego użytego podczas rzutowania.

    ```javascript
    var inputValue = '4';

    // źle
    var val = new Number(inputValue);

    // źle
    var val = +inputValue;

    // źle
    var val = inputValue >> 0;

    // źle
    var val = parseInt(inputValue);

    // dobrze
    var val = Number(inputValue);

    // dobrze
    var val = parseInt(inputValue, 10);
    ```

  - Jeżeli tworzysz akurat maszynę kwantową i niestety `parseInt` nie spełnia Twoich oczekiwań pod względem optymalizacji, posłuż się przesunięciem bitowym. [Są ku temu powody](http://jsperf.com/coercion-vs-casting/3), ale koniecznie pozostaw komentarz czemu to zrobiłeś.

    ```javascript
    // dobrze
    /**
     * parseInt sprawiało, że program lagował.
     * Przesunięcie bitowe Stringa koryguje
     * typ zmiennej i zachowuje szybkość działania.
     */
    var val = inputValue >> 0;
    ```

  - **Note:** Uważaj z rzutowaniem poprzez przesunięcie bitowe, jeżeli nie masz doświadczenia. Liczby przedstawione są w systemie jako [wartości 64-bitowe](http://es5.github.io/#x4.3.19), ale przesunięcia w JavaScript'cie zwracają zawsze 32-bitową liczbę ([źródło](http://es5.github.io/#x11.7)). Przesunięcia mogą przez to zachowywać się w dziwny sposób na liczbach przekraczających 32-bity ( [Dyskusja na temat](https://github.com/airbnb/javascript/issues/109) ). Największa liczba 32-bitowa to 2,147,483,647. Przykładowe przesunięcia i ich wyniki:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Rzutowanie Booleanów:

    ```javascript
    var age = 0;

    // źle
    var hasAge = new Boolean(age);

    // dobrze
    var hasAge = Boolean(age);

    // dobrze
    var hasAge = !!age;
    ```

**[⬆ do góry](#table-of-contents)**


## Nazwy zmiennych i funkcji

  - Unikaj jednoznakowych nazw. Staraj się w nazwie zawrzeć skrócony opis funkcjonalności tworzonej funkcji lub zmiennej.

    ```javascript
    // źle
    function q() {
      // ...kod...
    }

    // dobrze
    function query() {
      // ..kod..
    }
    ```

  - Używaj camelCase'a podczas nazywania zmiennych, funkcji i obiektów. Wyjątkiem są skróty składające się z kilku słów np. traverseDOM, getXHR itp.

    ```javascript
    // źle
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // dobrze
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Używaj PascalCase'a kiedy nadajesz nazwy konstruktorom i klasom.

    ```javascript
    // źle
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // dobrze
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Zmienne prywatne oznaczaj podkreśleniem `_`.

    ```javascript
    // źle
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // dobrze
    this._firstName = 'Panda';
    ```

  - Zapisuj referencję do zmiennej `this` jako `self`.

    ```javascript
    // źle
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // dobrze
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }
    ```

  - Nazywaj swoje funkcje, nawet jeżeli zapisujesz je do zmiennych. Pomaga to przy debuggowaniu.

    ```javascript
    // źle
    var log = function(msg) {
      console.log(msg);
    };

    // dobrze
    var log = function log(msg) {
      console.log(msg);
    };

    // źle
    $(this).on('listingUpdated', function(e, data) {
        // do sth
    });

    // dobrze
    $(this).on('listingUpdated', function updateListingWithSth(e, data) {
        // do sth
    });

    // dobrze
    (function myNamedIIFE() {
      // do sth
    })();
    ```

  - **Note:** IE8 i starsze wersje mogą się trochę dziwnie zachowywać, jeżeli zastosujesz powyższą zasadę. Więcej na temat [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/).

**[⬆ do góry](#table-of-contents)**


## Gettery i settery

  - Akcesory nie są wymagane przy dostępie do parametrów obiektów.
  - Jeżeli tworzysz akcesory używaj formatu getVal() i setVal('hello').

    ```javascript
    // źle
    dragon.age();

    // dobrze
    dragon.getAge();

    // źle
    dragon.age(25);

    // dobrze
    dragon.setAge(25);
    ```

  - Jeżeli zmienna do której się odnosisz to boolean, używaj isVal() lub hasVal().

    ```javascript
    // źle
    if (!dragon.age()) {
      return false;
    }

    // dobrze
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - get() i set() są w porządku, o ile używasz ich w czytelny sposób.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

**[⬆ do góry](#table-of-contents)**


## Konstruktory

  - Zamiast nadpisywać `prototype` nowym obiektem, dodawaj do niego pojedyncze metody. Nadpisywanie `prototype` sprawia, że dziedziczenie staje się niemożliwe, a prototypowanie traci swój sens.

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // źle
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // dobrze
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Zwracanie na końcu metod zmiennej `this` umożliwia stosowanie w kodzie chain'owania.

    ```javascript
    // źle
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // dobrze
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - Tworzenie własnej wersji konwertera toString() jest ok, jednak upewnij się, że Twoja metoda będzie działać w odpowiedni sposób i nie spowoduje bałąganu w kodzie.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ do góry](#table-of-contents)**


## Eventy

  - Wywołując event w aplikacji, utwórz nowy obiekt dla parametrów eventu i dopiero w nim dodaj zmienne, które chcesz wysłać. Dzięki temu kolejni programiści aplikacji będą mogli dodawać własne wartości do eventu, bez obaw, że nadpiszą Twoje zmienne. Przykładowo:

    ```js
    // źle
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // zrób coś z listingId
    });
    ```

    Zamiast tego lepiej jest używać tej formy:

    ```js
    // dobrze
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // zrób coś z data.listingId
    });
    ```

  - Event handlery powinny być definiowane w osobnych obiektach jako metody. Dzięki temu mogą on być ponownie użyte w innych miejscach.

  ```javascript
  // dobrze
      $(this).trigger('listingUpdated', { listingId : listing.id });

      ...

      function myFancyHandler (e, data) {
              // do something with data.listingId
      }

      ...

      $(this).on('listingUpdated', this.myFancyHandler);
  ```

  **[⬆ do góry](#table-of-contents)**


## <a name='moduly'></a> Moduły

  - Deklarację modułu rozpoczynaj znakiem `!`. Zapewnia to poprawne działanie modułu w każdych  warunkach, również po połączeniu skryptu z plikiem w którym, przykładowo, brakuje średnika na końcu innego modułu. [Wytłumaczenie](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933).
  - Pliki nazywaj stosując sposób zapisu camelCase. Moduły powinny znajdować się w plikach o odpowiadającej im nazwie, w odpowiadającym im folderom. Każdy moduł powinien również posiadać jedną, unikalną, globalną referencję - jeżeli jest ona potrzebna.
  - W modułach zawsze dodawaj metodę `noConflict()`, która odrzuca moduł i nadpisuje go innym, istniejącym w aplikacji odpowiednikiem.
  - Zawsze dodawaj 'use strict;' w nagłówkach swoich modułów.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[⬆ do góry](#table-of-contents)**


## jQuery

  - Nazwy zmiennych utworzonych przy pomocy jQuery rozpoczynaj znakiem dolara `$`.

    ```javascript
    // źle
    var sidebar = $('.sidebar');

    // dobrze
    var $sidebar = $('.sidebar');
    ```

  - Selektory jQuery bardzo często obciążają aplikację, ponieważ przeszukują struktury DOM w poszukiwaniu odpowiednich elementów. Aby zoptymalizować proces wyszukiwania, używaj wyników selektorów zapisanych w zmiennych, zamiast wywoływać ponownie metody jQuery.

    ```javascript
    // źle
    function setSidebar() {
      $('.sidebar').hide();

      // ...kod...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // dobrze
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...kod...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Ograniczaj zakres poszukiwań selektora jQuery przy pomocy elementów dziedziczących i dziedziczonych, tak bardzo jak tylko to możliwe, przykładowo: `$('.sidebar ul')` lub rodzic > dziecko: `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Używaj metody find() tylko na selektorach zapisanych wcześniej w zmiennych. W pozostałych przypadkach łącz całe wyrażenia w jeden selektor jQuery.

    ```javascript
    // źle
    $('ul', '.sidebar').hide();

    // źle
    $('.sidebar').find('ul').hide();

    // dobrze
    $('.sidebar ul').hide();

    // dobrze
    $('.sidebar > ul').hide();

    // dobrze
    $sidebar.find('ul').hide();
    ```

**[⬆ do góry](#table-of-contents)**


## Standard ECMAScript 5

  - Informacje dotyczące standardu: [Kangax](https://twitter.com/kangax/)  oraz [tabele kompatybilności](http://kangax.github.com/es5-compat-table/)

**[⬆ do góry](#table-of-contents)**


## Testowanie

  - **No raczej, nie inaczej.**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ do góry](#table-of-contents)**


## <a name='wydajnosc'></a> Wydajność (wszystkie artykuły w j. angielskim)

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ do góry](#table-of-contents)**


## <a name='wiecej-do-czytania'></a> Więcej do czytania ( również po angielsku )


**To koniecznie**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)


## Licencja

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ do góry](#table-of-contents)**

# };
