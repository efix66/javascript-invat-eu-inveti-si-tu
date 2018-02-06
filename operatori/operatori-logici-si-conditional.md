# Operatorii logici

Toate expresiile pot fi reduce la o valoare de adevăr, fie aceasta adevărată (`true`) sau falsă (`false`). Limbajul permite lucrul cu valorile de adevăr și falsitate pentru a decide un curs de evaluare sau pentru a proba anumite valori prin reducerea acestora la o concluzie înscrisă adevărului (și în engleză îi spunem *truthy*) sau falsității (în engleză îi spunem *falsey*).

## Operatorul de negare `!`

Acest operator este folosit atunci când dorești să inversezi evaluarea unei anumite valori din punct de vedere a valorii Boolean la cere se poate reduce.

```javascript
!1; // false
```

Ceea ce s-a întâmplat este că motorul JS a văzut operatorul și a evaluat valoarea de adevăr a lui 1. Știm că pentru 1, valoarea de adevăr este `true`. Negat, aceasta va deveni `false`.

La nevoie poți nega negația `!!true`.

## Operatorul logic ȘI - `&&` (AND)

Este un operator care spune firului de execuție că pentru a continua valorile de la stânga și de la dreapta operatorului, trebuie să fie evaluate la `true`. Doar dacă ambele expresii sunt evaluate la `true`, evaluarea poate continua. Dacă una dintre condiții este evaluată la `false`, întreaga evaluare este `false` iar execuția va urma traseul pregătit pentru parcursul `false`.

## Operatorul logic ORI - `||` (OR)

Folosirea acestui operator spune firului de execuție că cel puțin una dintre condiții trebuie să fie satisfăcută.

În cazul lui OR, JavaScript evaluează doar primul operand iar dacă acesta este *truthy*, nu va mai fi evaluat și cel de-al doilea operand. Evaluarea este pur și simplu scurt-circuitată pentru eficiență. Acest lucru se petrece pentru că doar evaluarea la `false` a operandului din stânga conduce la necesitatea de a evalua și pe cel de-al doilea operand pentru a da rezultatul final ca fiind `false` în cazul în care acesta este `false`.

```javascript
true || true;   // true
true || false;  // true
false || true;  // true
false || false; // false
```

Același mecanism este aplicat și în cazul lui AND (`&&`). În cazul lui OR, dacă valoarea primului operand se reduce la ceva fals, atunci este returnată valoarea celui de-al doilea operand.

```javascript
false && false; // false
false && true;  // false
true && false;  // false
true && true;   // true
```

Atenție la cazul în care poate ai dori evaluarea celui de-al doilea operand. Acest lucru nu se va întâmpla câtă vreme primul este *truthy*. Acest comportament poate fi considerat ca un mecanism selectiv de incrementare a unei variabile.

```javascript
var x = false, y = 0;
x || y++;
console.log(y);
```

Acest comportament permite o uzanță des întâlnită în programare pentru inițializarea unui obiect dacă acesta nu există deja. Acest idiom este întâlnit și în codul în care se dorește dotarea unei funcții cu valori pentru parametri chiar de la bun început.

```javascript
const obi = obi || {nume: "obiect important"};
```

În cazul nostru, obiectul `obi` ca operand este evaluat la o valoare `truthy` (toate obiectele la evaluare returnează `truthy`), dacă există deja, adică dacă a fost instanțiat. În acest caz literalul care instanția obiectul nu mai este evaluat.

## Operatorul ternar

Acest operator oferă posibilitatea analizei și reducerii expresiilor la valori boolene pentru a apuca diferite căi de acțiune. Spre deosebire de restul operatorilor care sunt fie unari, fie binari, acesta face evaluarea pentru trei operanzi. De aici îi vine și denumirea de operator ternar.

Are forma `expresie ? expresie pentru cazul true : expresie pentru cazul false`.

Primul operand de dinaintea semnului întrebării este cel care condiționează evaluarea celor care stau la dreapta și la strânga semnului două puncte. În cazul în care condiția este evaluată la o valoare truthy, prima expresie de după semnul întrebării va fi evaluată. În caz contrar, cea de după semnul două puncte.

```javascript
variabilă = 10 ? variabilă++ : 'am doar' + variabilă;
```

Mulți programatori folosesc acest operator pe post de sintaxă prescurtată a enunțului `if..else`, dar recomandarea generală este contrară acestei practici prin folosirea directă a enunțului.

Mai poți folosi operatorul ternar pentru inițializarea condiționată a unei variabile în funcție de o expresie.

```javascript
var x = true, y = 100;
var z = x ? y : 'eroare de inițializare';
console.log(z);
```