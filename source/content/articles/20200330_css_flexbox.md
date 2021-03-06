Title: Flexbox - wprowadzenie
Author: mkostyrko
Date: 2020-03-30 6:00
Updated:
Category: css
Tags: css, flexbox, pozycjonowanie-css
Slug: css-flexbox
related_posts: css-place-items

Flexbox jest technologią odpowiedzialną za układ treści znajdujących się na stronie internetowej w dosłownym tłumaczeniu można by to nazwać *elastycznym modelem pudełkowym*. Faktycznie ten sposób organizacji tzw layoutu lub też interfejsu oparty jest na równorzędnych **kontenerach** zwierających treść, które mogą być organizowane **w sposób poziomy (liniowy) lub pionowy (kolumnowy)** i pozwala na tworzenie **prostej i czytelnej struktury** strony i pozwala w relatywnie sposób na budowę **responsywnego projektu** strony internetowej.

Stosując flexboxa możemy łatwo:

* Pionowe i poziome wycentrować element względem rodzica
* Uszeregowanie elementów by wypełniały rodzica a odległość pomiędzy nimi była równa
* Elementy będą automatycznie zawijane w zależności od wielkości ekranu

Istotne jest to aby pamiętać, że dziecko rodzica zorganizowane w sposób flexboxowy również może być rodzicem dla własnego dziecka, którego pozycja będzie oparta na flexboxie

---

## Podstawowe deklaracje - rodzic

---

### display: flex

Zdefiniowanie układu opartego o flexboxa dokonujemy poprzez deklarację `display: flex` i oznacza to, że dzieci danego `div`'a (dzielnika/kontenera) będą zachowywały się w sposób *elastyczny*

Przykładowe zastosowanie

    div {
      display: flex;
    }


Podstawą zarządzania treścią opartą na flex-boxie są dwie osie **"pozioma" tzw. oś główna - ang. main axis** oraz **"pionowa" tzw. oś przecinające - ang. cross axis** 
(te są **zależne od** zdefiniowania **głównego kierunku organizacji projektu** przy pomocy **flex-direction** z podstawowego **liniowego** w **kolumnowy** i dlatego istotne jest aby nie przywiązywać się do kierunków **pionowy** czy **poziomy** bo są one relatywne)


![Schemat przedstawiający ogranizację flexboxa](https://developer.mozilla.org/files/3739/flex_terms.png)

Schemat przedstawiający ogranizację flexboxa

![Układ liniowy vs kolumnowy](https://css-tricks.com/wp-content/uploads/2018/10/flex-direction.svg)

Układ liniowy vs kolumnowy

---

### flex-direction

Deklaracja **dotycząca rodzica** i definiująca podstawowy układ zawartych w nim elementów (flex-items/dzieci)

`row` - układ liniowy/poziomy zaczynający się od lewej

`row-reverse` - układ poziomy zaczynający się od prawej

`column` - układ kolumnowy/pionowy zaczynający się od góry

`column-revers` - układ kolumnowy/pionowy zaczynający się od dołu

Przykładowe zastosowanie

    div {
      display: flex;
      flex-direction: row;
    }

---


### justify-content

Deklaracja wyrównująca elementy wzdłuż **głównej osi** (main-axis)

`flex-start` - obiekty są przyklejone **do  początku**(start)

`flex-end` - obiekty są przyklejone **do końca** (end)

`center` - obiekty są **wyśrodkowane** wzdłuż linii

`space-between` - obiekty są **równo rozdystrybuowane** w ramach rodzica i **przyklejone do granic**

`space-around` - obiekty są **równo rozdystrybuowane** w ramach rodzica z taką samą odległością **pomiędzy sobą jak i granicą** [pierwszy obiekt ma odległość w skali 1 po lewej ale 2 po prawej ponieważ sumuje się odległość obiektu 1. i 2.]

`space-evenly` - odległość pomiędzy każdym z **elementów i granic** jest równa

`start` / `end` / `right` / `left` - obiekty są zależne od [`writing-mode`](https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode) i uporządkowane kolejno od początku, końca, prawa, lewa

![justify-content](https://css-tricks.com/wp-content/uploads/2018/10/justify-content.svg){ width=50% }

---

### align-items

Deklaracja wyrównująca elementy wzdłuż **osi przecinającej** (cross-axis)

`flex-start` - elementy ułożone są przy górnej krawędzi rodzica (cross start)

`flex-end` - elementy ułożone są przy dolnej krawędzi rodzica (cross end)

`center` - wyśrodkowane wobec osi przecinającej

`stretch` - wypełnia cały kontener

`baseline` - wyśrodkowane wobec linii bazowej

![align-items](https://css-tricks.com/wp-content/uploads/2018/10/align-items.svg){ width=50% }

---

### align-content

Deklaracja wyrównująca elementy **wzdłuż głównej osi** (main-axis)

`flex-start` - element spakowane są do krawędzi górnej rodzica (cross-start)

`flex-end` - elementy spakowane są do krawędzi dolnej rodzica (cross-end)

`center` - elementy spakowane są do środka/ wycentrowane w ramach kontenera

`stretch` - elementy są rozciągnięte i wypełniają całą powierzchnię rodzica

`space-between` - elementy są równo rozdystrybuowane po kontenerze z równą przestrzenią pomiędzy sobą

`space-around` - elementy są równo rozdystrybuowane po kontenerze


![align-content](https://css-tricks.com/wp-content/uploads/2018/10/align-content.svg){ width=50% }

---

### flex-wrap

Deklaracja `flex-wrap` definiuje zawijanie się zawartych w rodzicu elementów.


  Przykładowe zastosowanie

    div {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
      }

Możliwe opcje: `nowrap` oraz `wrap-reverse`

Więcej na ten temat znajdziesz [tutaj](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-wrap)

---

### flex-flow
  flex-direction + flex-wrap = flex-flow np.

    div {
      flex-flow: column wrap;
    }

---


## Podstawowe deklaracje - dzieci (flex-items)

---

### align-self

Ta deklaracja pozwala na przedefiniowanie układu zdefiniowane z pozycji rodzica dla każdego elementu osobno -> patrz `align-items`

`flex-start`

`flex-end`

`center`

`baseline`

`stretch`

---

### flex-grow/shrink

  Definiuję współczynnik (factor) w jakim poszczególne elementy kurczą (grow) się lub rozrastają (shrink)

    .obiekt {
        flex-shrink: 1;
      }

    .obiekt:nth-of-type(2) {
      flex: 2;
    }

---

### flex-basis

`flex-basis` definiuje wielkość danego elementu, np.

    .obiekt {
      flex-basis: 20%;  <podstawową wartością jest: auto>
    }

---

### flex po raz kolejny

`flex-grow/shrink + flex-basis = flex`

Deklaracja `flex` w ramach dziecka może być użyta w kontekście potrzeby zdefiniowania jego wielkości oraz istotności w rozrastaniu-kurczeniu się wobec jego rodzeństwa - gdzie liczba na początku definiuje skalę a kolejna wielkość wyrażoną w px.
Przykładowe zastosowanie:

    .obiekt {
      flex: 1 200px;
    }

    .obiekt:nth-of-type(3) {
      flex: 2 200px;
    }


Skrótu `flex` można wykorzystać w następujący sposób

  .div1 {
      flex: 0 0 200px; // flex grow, shrink, basis
    }
  .div2 {
      flex: 1; // 0 grow; 1 shrink, basis: auto (całość)
    }

---

### inline-flex / inline flex-block

Display `inline-flex` lub `inline flex-box` odnosi się do kontenera a nie jego elementów - kolejno do wyświetlania w linii lub w formie blokowej

    .kontener {
      display: inline-flex;
    }

---

### order

  Definiuję kolejność w jakiej dany element się pojawia np. `order: -1` będzie oznaczał, że dany element *nadpisze* kolejność elementu zawartą w html i wyświetli go przed pierwszym elementem.

---

### Gry do ćwiczenia Flexboxa

http://www.flexboxdefense.com/

https://flexboxfroggy.com/

---

## Podsumowanie

Dla przypomnienia: istotne jest to aby pamiętać, że dziecko rodzica zorganizowane w sposób flexboxowy również może być rodzicem dla własnego dziecka, którego pozycja będzie oparta na flexboxie

![flexbox w flexboxie](https://mdn.mozillademos.org/files/13418/flexbox-example7.png)

Źródła (w tym grafik):

* [developer.mozilla.org](https://developer.mozilla.org/pl/docs/Learn/CSS/CSS_layout/Flexbox)

* [rejkowicz.pl](https://rejkowicz.pl/blog/co-to-jest-flexbox/)

* [css-tricks.com](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

* [grid.malven.co](http://grid.malven.co/)
