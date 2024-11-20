---
title: Брз почеток
---

<Intro>

Добредојдовте во документацијата React! Оваа страница ќе ве водеде во 80% од концептите на React што ќе ги користите секојдневно.

</Intro>

<YouWillLearn>

- Како да креирате и вгнездувате компоненти
- Како да додадете обележување(markup) и стилови
- Како да се прикажат податоци
- Како да рендерирате услови и списоци
- Како да одговорите на настани и да го ажурирате екранот
- Како да споделувате податоци помеѓу компонентите

</YouWillLearn>

## Создавање и вгнездување компоненти {/*components*/}

React апликациите се состојат од *компоненти*. Компонентата е дел од корисничкиот интерфејс (UI) што има своја логика и изглед. Компонентата може да биде мала како копче, или голема како цела страница.

React компонентите се JavaScript функции што враќаат маркап:

```js
function MyButton() {
  return (
    <button>Јас сум копче</button>
  );
}
```

Сегашто "MyButton" е прогласен, можете да го вгнездите во друга компонента:

```js {5}
export default function MyApp() {
  return (
    <div>
      <h1>Добредојдовте во мојата апликација</h1>
      <MyButton />
    </div>
  );
}
```

Забележете дека `<MyButton />` започнува со голема буква. Така знаете дека тоа е React компонента. Имињата на React компонентите секогаш мора да започнуваат со голема буква, додека HTML ознаките мора да бидат мали.

Погледнете го резултатот:

<Sandpack>

```js
function MyButton() {
  return (
    <button>
      Јас сум копче
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Добредојдовте во мојата апликација</h1>
      <MyButton />
    </div>
  );
}
```

</Sandpack>

Клучните зборови "export default" ја одредуваат главната компонента во фајлот. Ако не сте запознаени со некои делови од синтаксата на JavaScript, [MDN](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) и [javascript.info](https://javascript.info/import-export) имаат одлични референци.

## Пишување на маркап со JSX {/*writing-markup-with-jsx*/}

Синтаксата на маркапот што сте ја виделе погоре се нарекува *JSX*. Таа е опционална, но повеќето React проекти ја користат JSX поради нејзината удобност. Сите [алати што ги препорачуваме за локална развојна средина](/learn/installation) ги поддржуваат JSX без потреба од дополнителна конфигурација.

JSX е построг од HTML. Потребно е да ги затворите таговите, како на пример `<br />`. Вашата компонента исто така не може да врати повеќе JSX тагови. Потребно е да ги обвиете во заеднички родител, како на пример `<div>...</div>` или празен `<>...</>` обвивач:

```js {3,6}
function AboutPage() {
  return (
    <>
      <h1>За нас</h1>
      <p>Здраво тамо.<br />Како сте?</p>
    </>
  );
}
```

Ако имате многу HTML што треба да го конвертирате во JSX, можете да користите [онлајн конвертор.](https://transform.tools/html-to-jsx)

## Додавање стилови {/*adding-styles*/}

Во React, одредувате CSS класа со `className`. Работи на истиот начин како HTML атрибутот [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class):

```js
<img className="avatar" />
```

Потоа ги пишувате CSS правилата за него во посебен CSS фајл:

```css
/* Во вашиот CSS */
.avatar {
  border-radius: 50%;
}
```

React не пропишува како да додавате CSS фајлови. Во наједноставниот случај, ќе додадете [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) ознака во вашиот HTML. Ако користите алатка за градење или оквир, консултирајте се со документацијата за да научите како да додадете CSS фајлови во вашиот проект.

## Прикажување податоци {/*displaying-data*/}

JSX ви овозможува да ставите ознаки во JavaScript. Кадравите загради ви дозволуваат "да избегате назад" во JavaScript за да можете да вградите некоја варијабила од вашиот код и да ја прикажете на корисникот. На пример, ова ќе прикаже `user.name`:

```js {3}
return (
  <h1>
    {user.name}
  </h1>
);
```

Можете исто така да "избегате во JavaScript" од JSX атрибутите, но мора да користите кадрави загради *наместо* наводници. На пример, `className="avatar"` ја пренесува низата `"avatar"` како CSS класа, но `src={user.imageUrl}` ја чита вредноста на JavaScript варијабилата `user.imageUrl` и потоа ја пренесува таа вредност како атрибутот `src`:

```js {3,4}
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

Можете исто така да ставите посложени изрази во JSX кадравите загради, на пример, [спојување низи](https://javascript.info/operators#string-concatenation-with-binary):

<Sandpack>

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Слика на ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

```css
.avatar {
  border-radius: 50%;
}

.large {
  border: 4px solid gold;
}
```

</Sandpack>

Во горниот пример, `style={{}}` не е специјална синтакса, туку обичен објект `{}` во `style={ }` JSX кадравите заградите. Можете да го користите атрибутот "style" кога вашите стилови зависат од JavaScript варијабилите.

## Условно рендерирање {/*conditional-rendering*/}

Во React, нема специјална синтакса за пишување услови. Наместо тоа, се користат истите техники како и при пишувањето редовен JavaScript код. На пример, можете да користите [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) услов за условно вклучување на JSX:

```js
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

Ако преферирате по-компактно кодирање, можете да го користите [условниот оператор `?`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator). За разлика од `if`, тој работи внатре во JSX:

```js
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

Кога не ви е потребен `else` гранката, можете исто така да користите пократка [логичка `&&` синтакса](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation):

```js
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

Сите овие пристапи исто така функционираат за условно определување на атрибути. Ако не сте запознаени со некои од овие JavaScript синтакси, можете да започнете секогаш со користење на `if...else`.

## Рендерирање на листи {/*rendering-lists*/}

Ќе се потпрете на JavaScript функции како што се [`for` циклус](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) и [функцијата `map()` на низи](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) за рендерирање на листи од компоненти.

На пример, да речеме дека имате низата на производи:

```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
```

Внатре во вашата компонента, користете ја функцијата `map()` за да ја трансформирате низата на производи во низа од `<li>` елементи:

```js
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

Обратите внимание на тоа како `<li>` има атрибут `key`. За секој елемент во листата, треба да дадете израз или број што уникатно го идентификува тој елемент меѓу неговите соседи. Обично, клучот треба да доаѓа од вашите податоци, како што е ID од база на податоци. React ги користи вашите клучеви за да знае што се случило ако подоцна вметнете, избришете или пренаредите елементите.

<Sandpack>

```js
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

</Sandpack>

## Реагирање на настани {/*responding-to-events*/}

Можете да реагирате на настани со декларирање на *функции за обработка на настани* внатре во вашите компоненти:

```js {2-4,7}
function MyButton() {
  function handleClick() {
    alert('Сте кликнале на мене!');
  }

  return (
    <button onClick={handleClick}>
      Кликнете на мене
    </button>
  );
}
```

Обратите внимание на тоа како `onClick={handleClick}` нема загради на крајот! Не _викајте_ ја функцијата за обработка на настани: само треба да ја *пренесете*. React ќе ја повика вашата функција за обработка на настани кога корисникот ќе кликне на копчето.

## Ажурирање на екранот {/*updating-the-screen*/}

Често, ќе сакате вашата компонента да "запомни" некои информации и да ги прикаже. На пример, можеби сакате да ја броите бројката на кликнувања на копчето. За да го направите ова, додадете *статус(state)* на вашата компонента.

Прво, увезете [`useState`](/reference/react/useState) од React:

```js
import { useState } from 'react';
```

Сега можете да декларирате *променливата за статус(state)* во внатрешностра на вашата компонента:

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

Ќе добиете две работи од `useState`: актуелниот статус (`count`), и функцијата што ви овозможува да го ажурирате (`setCount`). Можете да им дадете било какви имиња, но конвенцијата е да пишувате `[something, setSomething]`.

При првото прикажување на копчето, `count` ќе биде `0` бидејќи му испративте `0` на `useState()`. Кога сакате да го промените статусот, повикајте ја `setCount()` и пренесете ја новата вредност. Кликнувањето на ова копче ќе го зголеми бројачот:

```js
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Кликнато {count} пати
    </button>
  );
}
```

React повторно ќе ја повика вашата функција за компонентата. Овој пат, `count` ќе биде `1`. Потоа ќе биде `2`. И така натаму.

Ако ја рендерирате истата компонента повеќе пати, секоја ќе има свој статус. Кликнете на секое копче одделно:

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Бројачи што се ажурираат одделно</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Кликнато {count} пати
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

Обратите внимание на тоа како секое копче "запомнува" својот `count` статус и не влијае на другите копчиња.

## Користење на Хуци {/*using-hooks*/}

Функциите што почнуваат со `use` се нарекуваат *Хуци*. `useState` е вградена функција (Hook) што ја нуди React. Можете да најдете други вградени Хуци во [API референцата.](/reference/react) Исто така, можете да напишете свои Хуци со комбинирање на постоечките.

Хуците се по рестриктивни од другите функции. Можете да повикате Хуци *само на врвот* на вашите компоненти (или другите Хуци). Ако сакате да користите `useState` во услов или циклус, извлечете нова компонента и ставете ја таму.

## Споделување податоци помеѓу компоненти {/*sharing-data-between-components*/}

Во претходниот пример, секој `MyButton` имаше свој независен `count`, и кога секое копче беше кликнато, само `count` за кликнатото копче се смени:

<DiagramGroup>

<Diagram name="sharing_data_child" height={367} width={407} alt="Дијаграм што прикажува дрво од три компоненти, еден родител обележан како MyApp и два деца обележани како MyButton. И двете компоненти MyButton содржат count со вредност нула.">
 
Првично, секоја `MyButton`'s `count` статус е `0`

</Diagram>

<Diagram name="sharing_data_child_clicked" height={367} width={407} alt="Истиот дијаграм како претходниот, со истакнат count на првата компонента MyButton што укажува на клик со вредноста на count зголемена на еден. Втората компонента MyButton сè уште содржи вредност нула.">

Првата `MyButton` го ажурира својот `count` на `1`

</Diagram>

</DiagramGroup>

Сепак, често ќе биде потребно компонентите да *споделуваат податоци и секогаш да се ажурираат заедно*.

За да направите и двете компонентов `MyButton` да прикажуваат истиот `count` и да се ажурираат заедно, треба да го преместите статусот од поединечните копчиња "повисоко" до најблиската компонента што ги содржи сите нив.

Во овој пример, тоа е `MyApp`:

<DiagramGroup>

<Diagram name="sharing_data_parent" height={385} width={410} alt="Дијаграм што прикажува дрво од три компоненти, еден родител обележан како MyApp и два деца обележани како MyButton. MyApp содржи вредност на count нула што се пренесува до двете компоненти MyButton, кои исто така покажуваат вредност нула.">

Првично, статусот на `MyApp` е `0` и се пренесува до двата деца

</Diagram>

<Diagram name="sharing_data_parent_clicked" height={385} width={410} alt="Истиот дијаграм како претходниот, со истакнат count на родителот MyApp што укажува на клик со вредноста зголемена на еден. Текот до двете деца MyButton е исто така истакнат, а вредноста на count во секое дете е поставена на еден, што укажува на тоа дека вредноста е пренесена.">

При клик, `MyApp` го ажурира својот статус на `count` на `1` и го пренесува до двете деца

</Diagram>

</DiagramGroup>

Сега, кога ќе кликнете на било кое копче, `count` во `MyApp` ќе се промени, што ќе ги промени и двата `count` во `MyButton`. Еве како можете да го изразите тоа во код.

Прво, *преместете го статусот* од `MyButton` во `MyApp`:

```js {2-6,18}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Бројачи што се ажурираат одделно</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... преместуваме код од тука ...
}
```

Потоа, *пренесете го статусот* од `MyApp` до секој `MyButton`, заедно со споделената функција за обработка на кликови. Можете да пренесете информации до `MyButton` користејќи ги фигурните загради на JSX, исто како што го правевте претходно со вградените тагови како `<img>`:

```js {11-12}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Бројачи што се ажурираат заедно</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

Информацијата што ја пренесувате на овој начин се нарекува _props_. Сега компонентата `MyApp` содржи статус на `count` и функцијата за обработка на кликови `handleClick`, и *ги пренесува и двете како props* до секое од копчињата.

На крајот, променете ја `MyButton` за да *прочита* props што ги пренесовте од нејзиниот родител:

```js {1,3}
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Кликнато {count} пати
    </button>
  );
}
```

Кога ќе кликнете на копчето, функцијата за обработка на `onClick` ќе се активира. `onClick` проп на секое копче беше поставен на функцијата `handleClick` внатре во `MyApp`, па кодот во неа се извршува. Тоа код повикува `setCount(count + 1)`, зголемувајќи ја променливата за статус `count`. Новата вредност на `count` се пренесува како проп до секое копче, така што сите прикажуваат нова вредност. Ова се нарекува "подигање на статусот". Со преместување на статусот нагоре, го споделивте помеѓу компонентите.

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Бројачи што се ажурираат заедно</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Кликнато {count} пати
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

## Следни чекори {/*next-steps*/}

Сега, веќе ги знаете основите на тоа како да пишувате React код!

Погледнете го [Училиштето](/learn/tutorial-tic-tac-toe) за да ги примените и да изградите ваша прва мини-апликација со React.