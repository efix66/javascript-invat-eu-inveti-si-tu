# Funcții async / await

Acest enunț a fost introdus în ECMAScript 2017 și face ca o funcție să returneze o promisiune (`Promise`). Funcțiile care au cuvântul cheie `async` în față, vor returna mereu o promisiune.

```javascript
async function facCeva () {};
async () => console.log;
```

**Moment Zen**: Valoarea returnată de o funcție `async` este împachetată în `Promise.resolve()`.

Apariția lor marchează o nouă paradigmă de lucru cu promisiunile.

```javascript
async function cevaAsincron () {
  return 'apar asincron';
  // echivalentul lui return Promise.resolve('apar asincron');
};
cevaAsincron().then(console.log);
```

În exemplul de mai sus, valoarea 'apar asincron' va fi împachetată într-o promisiune datorită folosirii în subsidiar al lui `Promise.resolve()`.

Avantajul este evident în cazul în care scrii o bibliotecă de cod sau o funcție cu rol utilitar în care nu știi dacă datele care vin sunt *sync* sau *async*. Atunci când trebuie returnat un obiect, acesta va fi returnat fără împachetare.

Înainte de apariția acestor funcții, pentru a scrie cod asincron aveam la îndemână callback-uri și promisiuni. În mod curent, până la apariția async/await ai fi returnat un obiect `Promise` proaspăt instanțiat.

```javascript
function simulare () {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("Salutare"), 2000);
  });
};
let x = simulare();
// Promise {[[PromiseStatus]]: "pending", [[PromiseValue]]: undefined}
// după 2 secunde
// [[PromiseValue]]: "Salutare"
```

Aceste noi funcții te fac să percepi asicronicitatea ceva mai secvențial, mai sincron. O funcție `async` returnează o promisiune. Cheia utilizării funcțiilor `async` este utilizarea operatorului `await`. Acest operator poate fi folosit în interiorul unei expresii de funcție `async` și numai în interiorul unei astfel de funcții. Acesta returnează valoarea rezolvată a unei promisiuni. În același timp, operatorul oprește execuția funcției `async` până când promisiunea este rezolvată. Abia după rezolvarea promisiunii, va fi reluată execuția funcției. Dacă valoarea nu este o promisiune, este convertită la o promisiune rezolvată. În cazul în care promisiunea este respinsă, este returnată valoarea rezultată ca respingere.

```javascript
async function sarcina () {
  let salut = await simulare();
  return `${salut}`;
};
sarcina().then(mesaj => console.log(mesaj));
```

Înțelegând aceste aspecte, ajungem la concluzia că avantajul pe care îl oferă funcțiile `async`/`await` este legat de posibilitatea de a introduce o ordine în care să apară rezultatele datorită întreruperii execuției funcției și implicit a soluționării celorlalte prin efectul lui `await`, chiar dacă acestea sunt extrase într-o manieră asicronă. Pur și simplu oferă posibilitatea de a ordona succesiunea operațiunilor similar rulării sincrone a codului.

Luând în considerare și cunoștințele de la promisiuni, ia-ți un moment de reflecție pentru a decide în codul pe care îl scrii care este cea mai bună soluție de a soluționa promisiunile: secvențial, folosind `async`/`await`, paralel, folosind `Promise.all` sau prima din toate cele evaluate folosind `Promise.race`.

## Declarare

În ceea ce privește sintaxa, vor fi folosite în tandem două cuvinte cheie: `async` în deschiderea declarației și `await` în corpul funcției.

```javascript
// declarație de funcție
async numeFunctie () {
  await ...
};
// funcție săgeată
var numeFunctie = async () => {
  await ...
};
```

Funcțiile `async` pot îndeplini rol de metode în obiecte, putând servi și claselor. În cazul claselor, constructorii, setter-ii și getter-ii nu pot fi async.

```javascript
var obi = {
  async numeFunctie () {
    await ...
  }
};
class Ceva {
  async numeFunctie () {
    await ...
  }
}
```

## Operatorul `await`

Funcțiile `async` își vor întrerupe execuția ori de câte ori întâlnesc `await` în corp. Cuvântul cheie `await` își produce efectele doar dacă este folosit în corpul funcțiilor `await`.

Operatorul `await` expune o deficiență a acestor funcții. Ceea ce se petrece atunci când îl folosim este oprirea din execuție a întregului cod până când promisiunile sunt **rezolvate**. Aceasta este o caracteristică de execuție pe care o întâlnim la rularea codului sincron. Chiar dacă alte sarcini își urmează cursul, propriul cod este blocat în execuție.

## Exemplu

Să lucrăm cu un caz ceva mai util. Atunci când am discutat despre cum este executat codul JavaScript, am folosit un exemplu.

```javascript
// înlocuiește cheia API din link numită wskey, cu una personală obținută
// de la https://pro.europeana.eu/get-api
// Secvența unde vei pune cheia ta este wskey=XXXXXXXXX
// înlocuiește XXXXXXXXX cu propria cheie
// dacă nu introduci cheia personală vei avea o eroare
// Cross-Origin Request Blocked
let adresa = "https://www.europeana.eu/api/v2/search.json?wskey=XXXXXXXXX&query=The%20Fraternity%20between%20Romanian%20and%20French%20Army";
function aduMi () {
  fetch(adresa)
    .then((resursa) => resursa.json())
    .then((înregistrarea) => console.log(înregistrarea))
    .catch(() => console.log('A apărut o eroare'));
};
aduMi();
```

Pentru a simplifica, putem transforma funcția noastră într-una `async`.

```javascript
let adresa = "https://www.europeana.eu/api/v2/search.json?wskey=XXXXXXXXX&query=The%20Fraternity%20between%20Romanian%20and%20French%20Army";
async function aduMi () {
  const resursa = await fetch(adresa);
  const rezultat = await resursa.json();
  return rezultat;
};
const promisiunea = aduMi();
typeof promisiune;
aduMi().then((înregistrarea) => console.log(înregistrarea)).catch(() => console.log('A apărut o eroare'));
```

Ceea ce permit funcțiile `async`/`await` este posibilitatea de a captura toate erorile ridicate de codul asincron, dar și cel sincron. Deci, folosirea unei structuri `try...catch` este valabilă.

## Gestionarea erorilor

Unul din avantajele majore ale folosirii sintaxei `async`/`await` este aceea că permite evaluarea codului în structuri `try...catch`.

```javascript
async function ceva () {
  try {
    var date = await fetch('https://radacina.ro/cale_catre_set_de_date');
  } catch (error) {
    console.log(error);
  }
};
```

Erorile care ar putea apărea în urma evaluării unei expresii `throw new Error('O eroare la evaluare')` din funcție. Acestea for putea fi gestionate în `catch(error){}`.

## Concluzii

Scrierea codului asyncron folosind `async`/`await` este o alternativă elegantă și mult mai eficientă (cel puțin în cazul detectării erorilor) pentru lucrul cu promisiunile. Totuși nu este un înlocuitor pentru promisiuni, ci un model mai eficient de folosire a acestora. De exemplu, pentru a rezolva în paralel promisiuni, vom apela `Promise.all()`, dar rezultatul poate fi gestionat cu un `async`.

```javascript
const [val1, val2, val3] = await Promise.all(promise1(), promise2(), promise3())
```

## Dependințe cognitive

- iteratori
- generatoare
- promisiuni

## Referințe

- [Asynchronous Adventures in JavaScript: Async/Await](https://medium.com/dailyjs/asynchronous-adventures-in-javascript-async-await-bd2e62f37ffd)
- [await. MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)
- [7 Reasons Why JavaScript Async/Await Is Better Than Plain Promises (Tutorial)](https://dev.to/gafi/7-reasons-to-always-use-async-await-over-plain-promises-tutorial-4ej9)
- [Async functions - making promises friendly | Google Developers](https://developers.google.com/web/fundamentals/primers/async-functions)
- [Easier Node.js streams via async iteration](https://2ality.com/2019/11/nodejs-streams-async-iteration.html)
- [Making asynchronous programming easier with async and await | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
