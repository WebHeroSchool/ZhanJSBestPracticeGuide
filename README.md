# JavaScript Best Practice Guide
## ЛУЧШИЕ И УЖЕ ДАВНО УСТОЯВШИЕСЯ ПРАКТИКИ

### 1. Используйте `const` для объявления переменных; избегайте `var`. eslint: `prefer-const`, `no-const-assign`
> Почему? Это гарантирует, что вы не сможете переопределять значения, т.к. это может привести к ошибкам и к усложнению понимания кода.
``` js
// плохо
var a = 1;
var b = 2;

// хорошо
const a = 1;
const b = 2;
```

### 2. Для создания объекта используйте литеральную нотацию. eslint: `no-new-object`
``` js
// плохо
const item = new Object();

// хорошо
const item = {};
```

### 3. Используйте вычисляемые имена свойств, когда создаёте объекты с динамическими именами свойств.
> Почему? Они позволяют вам определить все свойства объекта в одном месте.
``` js
function getKey(k) {
  return `a key named ${k}`;
}

// плохо
const obj = {
  id: 5,
  name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// хорошо
const obj = {
  id: 5,
  name: 'San Francisco',
  [getKey('enabled')]: true,
};
```

### 4. Используйте сокращённую запись метода объекта. eslint: `object-shorthand`
``` js
// плохо
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// хорошо
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```

### 5. Используйте сокращённую запись свойств объекта. eslint: `object-shorthand`
> Почему? Это короче и понятнее.
``` js
const lukeSkywalker = 'Luke Skywalker';

// плохо
const obj = {
  lukeSkywalker: lukeSkywalker,
};

// хорошо
const obj = {
  lukeSkywalker,
};
```

### 6. Группируйте ваши сокращённые записи свойств в начале объявления объекта.
> Почему? Так легче сказать, какие свойства используют сокращённую запись.
``` js
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// плохо
const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
};

// хорошо
const obj = {
  lukeSkywalker,
  anakinSkywalker,
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
};
```

### 7. Только недопустимые идентификаторы помещаются в кавычки. eslint: `quote-props`
> Почему? На наш взгляд, такой код легче читать. Это улучшает подсветку синтаксиса, а также облегчает оптимизацию для многих JS-движков.
``` js
// плохо
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// хорошо
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```

### 8. Не вызывайте напрямую методы `Object.prototype`, такие как `hasOwnProperty`, `propertyIsEnumerable`, и `isPrototypeOf`. eslint: `no-prototype-builtins`
> Почему? Эти методы могут быть переопределены в свойствах объекта, который мы проверяем `{ hasOwnProperty: false }`, или этот объект может быть `null` (`Object.create(null)`).
``` js
// плохо
console.log(object.hasOwnProperty(key));

// хорошо
console.log(Object.prototype.hasOwnProperty.call(object, key));

// отлично
const has = Object.prototype.hasOwnProperty; // Кэшируем запрос в рамках модуля.
console.log(has.call(object, key));
/* или */
import has from 'has'; // https://www.npmjs.com/package/has
console.log(has(object, key));
```

### 9. Используйте оператор расширения вместо `Object.assign` для поверхностного копирования объектов. Используйте синтаксис оставшихся свойств, чтобы получить новый объект с некоторыми опущенными свойствами.
``` js
// очень плохо
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // эта переменная изменяет `original` ಠ_ಠ
delete copy.a; // если сделать так

// плохо
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// хорошо
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```

### 10. Для создания массива используйте литеральную нотацию. eslint: `no-array-constructor`
``` js
// плохо
const items = new Array();

// хорошо
const items = [];
```