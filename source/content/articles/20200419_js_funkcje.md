Title: JavaScript - funkcje
Author: mkostyrko
Date: 2020-04-19 10:00
Updated:
Category: javascript
Tags: javascript, js, funkcja
Slug: js-funkcja
related_posts: js-funkcja-starzalkowa

### Funkcje

Zbiór zgrupowanych instrukcji, które można wywołać poprzez odwołanie się do ich nazwy, funkcja może ale nie musi przyjmować parametry/argumenty, które biorą udział w wywoływaniu się jej wewnętrznego kodu. 

Funkcję można stworzyć poprzez jej deklarację przy użycia słowa kluczowego `function` 

Schemat 

    function identyfikator(argument) {
      kod;
    }
    // wywołanie
    identyfikator(argument)

::: Funkcje posiadają własne właściwości np. .name -> identyfikator.name = identyfikator (może być użyty w funkcji) :::

**Hoisting/wynoszenie** - funkcje tak samo jak i deklaracje zmiennych w trakcie analizowania/parsowania kodu przez przeglądarkę są wynoszone na górę aktualnego zasięgu i tam pojawiają się w zapisanej kolejności (ma to wpływ na nadpisywanie) ->> `Można wywoływać funkcje zanim zostaną zadeklarowane`. 

::: wynoszenie odbywa się przed wykonaniem kodu :::


Istnieje możliwość zdefiniowania domyślnego parametru w funkcji, który zostaje użyty w przypadku gdy ten nie zostanie podany (przy pomocy operatora `=` po którym wpisujemy domyślną wartość)

    function droidSeeker (name = 'droid') {
      console.log(`This is not the ${name} you are looking for`)
    }

    droidSeeker('Mike')
    >> This is not the Mike you are looking for
    droidSeeker()
    >> This is not the droid you are looking for

---

### Funkcje pomocnicze

Funkcje pomocnicze pozwalają na rozbicie kodu na jego poszczególne części tak aby był czytelniejszy i prostszy w utrzymaniu

w tym celu można stworzyć jedną funkcję cząstkową a następnie odwołać się do niej w kolejnej

    function multipNineFifth (num) {
      return num * (9/5); 
    }

    function getFarenheit(celc) {
      return multipNineFifth(celc) + 32;
    }

    getFarenheit(10) // = >50


----

### Wyrażenia funkcyjne

Funkcje (zwykle anonimowa) można przypisać do zmiennych  - wówczas: 

    const zmienna = function(arg1,arg2) {
      return arg1 + arg2;
    }
    // wywołanie
    console.log(zmienna(1,2));

W ten sposób zadeklarowana funkcja nie jest **wynoszona** 

::: wynoszona jest deklaracja funkcja jeśli jest poprzedzona  słowem kluczowego **`var`**:::

Przypadek z nazwaną funkcją

    const zmienna = function nazwa(x,y) {
      return x+y;
    }

    console.log(zmienna.name); // = nazwa -> pozwala na odwołanie się do f(x) w jej wnętrzu

Funkcja w domyśle zwraca wartość `undefined` - stąd aby zwróciła jakąś wartość należy zastosować instrukcję `return` ::: **, który przerywa również działanie funkcji** :::


---
### Funkcje anonimowe 

Nie posiada nazwy (identyfikatora), może ale nie musi być przypisana do zmiennej, może ale nie musi posiadać argumentu // występuje często jako funkcja występująca jako parametr w innej
Gdy przypisana do zmiennej wywołuje się poprzez jej przywołanie w innym przypadku należy zamknąć ją w nawiasach -> funkcja zostanie od razu wywołana w trakcie wykonywania kodu.

    (function(arg){
      console.log(arg)
    })("Argument");

---

### Funkcja natychmiastowa Tzw IIFE (Immediately-Invoked Function Expression)

Funkcja, która jest deklarowana i przeprowadzana "w tym samym czasie" - taka funkcja zamknięta jest w nawias (często jest anonimowa) i jest natychmiast wywoływana. Często wykorzystywana w metodach tablic.

    (function(droid){
      console.log('this is ' + droid);
    })('r2d2');
    >> this is r2d2

    for(let i = 0; i < 12; i++) {
          setTimeout(function() {
          console.log(i);
          }, 500); 
    }

Pozwala na uniknięcie zdublowania nazw zmiennych. Nie trzeba osobno wywoływać funkcji. 

---
Źródła:

http://kursjs.pl/kurs/super-podstawy/funkcje.php

http://kursjs.pl/kurs/super-podstawy/funkcje-tematy-dodatkowe.php#callback

http://jsdn.pl/samowywolujace-sie-anonimowe-funkcje/

http://jsdn.pl/funkcje-tworzenie-funkcji-w-javascripcie/

https://www.modestprogrammer.pl/wyrazenia-funkcyjne-oraz-funkcje-anonimowe-w-javascript

http://papoldesign.pl/edukacja/javascript-funkcja-natychmiastowa-iife/