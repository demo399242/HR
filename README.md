- - -
**Роман Кайрис**
_+7 (775) 224-70-12_ (whatsapp, telegram)
- - -

## Question 1
You've been tasked with identifying a string that contains the word "superman" (case insensitive). You've written the following code:
```js
function validateString(str) {
    if (!str.toLowerCase().indexOf('superman')) {
        throw new Error('String does not contain superman');
    }    
}
```

QA has come to you and said that this works great for strings like "I love superman", but an exception is generated for strings like "Superman is awesome!", which should not happen. Explain why this occurs, and show how you would solve this issue (you must use `indexOf()` in your answer).

#### ОТВЕТ:

Если подстрока находится в самой первой позиции, то indexOf() возвращает 0, что является falsy-значением.

**Решение:** проверка на -1,
```js
if (str.toLowerCase().indexOf('superman') == -1) {
  throw new Error('String does not contain superman');
}  
``` 
либо достаточно _древний_ способ, через тильду **~** (побитовая инверсия, -1 перейдет в 0, другие значения в ненулевые)
```js
if (!~str.toLowerCase().indexOf('superman')) {
  throw new Error('String does not contain superman');
}  
``` 

## Question 2
You're given a sorted index array that contains no keys. The array contains only integers, and your task is to identify whether or not the integer you're looking for is in the array. Write a function that searches for the integer and returns true or false based on whether the integer is present. Describe how you arrived at your solution.

#### ОТВЕТ:

Линейный проход отсортированному массиву длины N, даст нам в среднем N/2 операций сравнения.

Можно воспользоваться методом бинарного поиска, это даст нам log(N) по основанию 2 операций сравнения, что при больших N будет ощутимо быстрее.

**Рекурсивное решение:**

```js
function helper (arr, x, l, r, desc) {
  if (l >= r) return false
  const m = Math.floor(l + (r - l) / 2)
  if (arr[m] == x) return true
  else if (arr[m] > x && !desc || arr[m] < x && desc)
    return helper(arr, x, l, m, desc)
  else
    return helper(arr, x, m+1, r, desc)
}

function isPresent (arr, x) {
  if (arr.length == 0) return false
  const desc = arr[0] > arr[arr.length-1]
  return helper(arr, x, 0, arr.length, desc)
}
```

## Question 3
Write a function that takes a phone number in any form and formats it using a delimiter supplied by the developer. The delimiter is optional; if one is not supplied, use a dash (-). Your function should accept a phone number in any format (e.g. 123-456-7890, (123) 456-7890, 1234567890, etc) and format it according to the 3-3-4 US block standard, using the delimiter specified. Assume foreign phone numbers and country codes are out of scope.

*Note:* This question CAN be solved using a regular expression, but one is not REQUIRED as a solution. Focus instead on cleanliness and effectiveness of the code, and take into account phone numbers that may not pass a sanity check.

#### Ответ:

Как я понял:
1. не используем регулярные выражения;
2. в качестве входных данных может быть что угодно;
3. номер верный, если на вход передана строка в которой ровно 10 цифр (US номер);

**Решение:** 

```js
function validatePhone (rawPhone, delim="-") {

  // Если не строка, то исключение
  if (typeof rawPhone !== "string")
    throw new Error("Входное значение не является строкой")
  
  // Оставляем только цифры
  const phone = rawPhone.split("").filter(c => c>="0" && c<="9").join("")

  // Если длина строки не равна 10 (US-number), то исключение
  if (phone.length != 10)
    throw new Error("В номере должно быть ровно 10 цифр")

  // Формируем результат в требуемом формате
  return phone.slice(0, 3) + delim + phone.slice(3, 6) + delim + phone.slice(6)

}
```

## Question 4
Write a complete set of unit tests for the following code:

```js

function fizzBuzz(start = 1, stop = 100)
{
    let result = '';

    if (stop < start || start < 0 || stop < 0) {
        throw new Error('Invalid arguments');
    }

    for (let i = start; i <= stop; i++) {
        if (i % 3 === 0 && i % 5 === 0) {
            result += 'FizzBuzz';
            continue;
        }

        if (i % 3 === 0) {
            result += 'Fizz';
            continue;
        }

        if (i % 5 === 0) {
            result += 'Buzz';
            continue;
        }

        result += i;
    }

    return result;
}
```

#### Ответ:

```js
describe("Test for fizzBuzz()", function() {

  const { fizzBuzz } = require("./func")

  test('Invalid arguments: stop<start', () => {
    expect(() => {
      fizzBuzz(2, 1)
    }).toThrow("Invalid arguments")
  });

  test('Invalid arguments: start<0', () => {
    expect(() => {
      fizzBuzz(-1, 0)
    }).toThrow("Invalid arguments")
  });

  test('Invalid arguments: stop<0', () => {
    expect(() => {
      fizzBuzz(1, -1)
    }).toThrow("Invalid arguments")
  });

  test('Range[0, 10]', () => {
    expect(fizzBuzz(0, 10)).toBe('FizzBuzz12Fizz4BuzzFizz78FizzBuzz');
  });

  test('Range[50, 60]', () => {
    expect(fizzBuzz(50, 60)).toBe('BuzzFizz5253FizzBuzz56Fizz5859FizzBuzz');
  });

  test('Range[90, 100]', () => {
    expect(fizzBuzz(90, 100)).toBe('FizzBuzz9192Fizz94BuzzFizz9798FizzBuzz');
  });

  test('Range[990, 1000]', () => {
    expect(fizzBuzz(990, 1000)).toBe('FizzBuzz991992Fizz994BuzzFizz997998FizzBuzz');
  });

});
```

## Question 5
I have an array of file names:

```js
const files = [
    '/src/common/api.js',
    '/src/common/preferences.js',
    '/src/styles/main.css',
    '/src/styles/base/_base.scss',
    '/src/assets/apple-touch-icon-57.png',
];
```

Below is a mixed list of specific file names, as well as file paths, that should be excluded. For example, ALL files under a file path should be excluded, but if the value is an actual file name, only that specific file should be excluded. 

```js
const exclude = [
    '/src/styles/',
    '/src/assets/apple-touch-icon-57.png',
];
```

For example, you'll want to exclude the styles directory (and all files in it), but ONLY the `apple-touch-icon-57.png` file.

Devise a method for applying the exclusion list against the file list WITHOUT nested `forEach()` loops.

#### Ответ:

```js

const isDirectory = path => path.slice(-1)==="/"

const excludeFilter = (items) => file => {
  let result = true
  items.forEach(item => {
    const isDir = isDirectory(item)
    if (
      (!isDir && item===file) || 
      (isDir && file.startsWith(item)) 
    ) result = false
  })
  return result
}

const files = [
  '/src/common/api.js',
  '/src/common/preferences.js',
  '/src/styles/main.css',
  '/src/styles/base/_base.scss',
  '/src/assets/apple-touch-icon-57.png',
];

const exclude = [
  '/src/styles/',
  '/src/assets/apple-touch-icon-57.png',
];

const filteredFiles = files.filter(excludeFilter(exclude))
console.log(filteredFiles)
```

## Question 6

There are two tables in Postgres database:
user(id integer, email varchar(256), first_name varchar(256), last_name varchar(256));
payment(id integer, user_id integer, amount integer, successful boolean, created_at timestamp with time zone);

Write database queries:
1. to select emails for users, which have successful payments
2. to select emails for users, which have only unsuccessful payments
3. to select count and sum of successful payments for each user
4. to select last(by creation date) 5 payments for each user

What constraints need these tables for data consistency?

#### Ответ:

1.
```sql
SELECT DISTINCT u.email 
FROM user u
INNER JOIN payment p ON u.id = p.user_id 
WHERE p.successful;
```
2.
Под эту ситуацию попадают и те пользователи у которых вообще нет платежей (любых).
```sql
SELECT *
FROM user u
WHERE u.id NOT IN (
  SELECT DISTINCT user_id
  FROM payment
  WHERE successful
)
ORDER BY u.email
```
3.

Если нужно показать *ВСЕХ* пользователей у которых *ЕСТЬ* успешные платежи, то
```sql
SELECT u.email, COUNT(*), SUM(p.amount) 
FROM user u
INNER JOIN payment p ON u.id = p.user_id
WHERE p.successful
GROUP BY u.email
```
Если же нужно показать *ВСЕХ* пользователей, даже тех у которых *НЕТ* успешных платежей, то
```sql
SELECT u.email, COALESCE(SUM(p.amount),0), COUNT(p.id)
FROM user u
LEFT JOIN payment p ON u.id = p.user_id AND p.successful
GROUP BY u.email
```
4.
```sql
SELECT * 
FROM (
  SELECT u.id as user_id, email, first_name, last_name, p.id as payment_id, created_at, amount, successful,
  RANK() OVER (PARTITION BY u.id ORDER BY p.created_at DESC) AS rk
  FROM payment p
  JOIN user u ON p.user_id = u.id
) t
WHERE  rk <= 5
ORDER BY email
```

## Question 7

There is array of objects

```js
const queryResult = [
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 1, bookTitle: 'don quixote'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 3, bookTitle: 'robinson crusoe'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 4, bookTitle: 'gulliver\'s travels'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 5, bookTitle: 'frankenstein'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 6, bookTitle: 'the count of monte cristo'},

  { userId: 2, firstName: 'richard', lastName: 'roe', bookId: 2, bookTitle: 'pilgrim\'s progress'},
  { userId: 2, firstName: 'richard', lastName: 'roe', bookId: 6, bookTitle: 'the count of monte cristo'},


  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 1, bookTitle: 'don quixote'},
  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 2, bookTitle: 'pilgrim\'s progress'},
  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 5, bookTitle: 'frankenstein'},
  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 7, bookTitle: 'jane eyre'},
];
```

How to map it to
```js
const mappedResult = [{
  id: 1,
  firstName: "John",
  lastName: "Doe",
  books: [
      {
        id: 1,
        title: "Don Quixote"
      },
      {
        id: 3,
        title: "Robinson Crusoe"
      },
      {
        id: 4,
        title: "Gulliver's Travels"
      },
      {
        id: 5,
        title: "Frankenstein"
      },
      {
        id: 6,
        title: "The Count Of Monte Cristo"
      }
    ]
}, {
    id: 2,
    firstName: "Richard",
    lastName: "Roe",
    books: [
      {
        id: 2,
        title: "Pilgrim's Progress"
      },
      {
        id: 6,
        title: "The Count Of Monte Cristo"
      }
    ]
}, {
  id: 3,
  firstName: "John",
  lastName: "Smith",
  books: [
      {
        id: 1,
        title: "Don Quixote"
      },
      {
        id: 2,
        title: "Pilgrim's Progress"
      },
      {
        id: 5,
        title: "Frankenstein"
      },
      {
        id: 7,
        title: "Jane Eyre"
      }
    ]
}];
```
?
Notice: You can use lodash in this task.

#### Ответ:

```js
import _ from 'lodash';

const queryResult = [
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 1, bookTitle: 'don quixote'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 3, bookTitle: 'robinson crusoe'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 4, bookTitle: 'gulliver\'s travels'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 5, bookTitle: 'frankenstein'},
  { userId: 1, firstName: 'john', lastName: 'doe', bookId: 6, bookTitle: 'the count of monte cristo'},
  { userId: 2, firstName: 'richard', lastName: 'roe', bookId: 2, bookTitle: 'pilgrim\'s progress'},
  { userId: 2, firstName: 'richard', lastName: 'roe', bookId: 6, bookTitle: 'the count of monte cristo'},
  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 1, bookTitle: 'don quixote'},
  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 2, bookTitle: 'pilgrim\'s progress'},
  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 5, bookTitle: 'frankenstein'},
  { userId: 3, firstName: 'john', lastName: 'smith', bookId: 7, bookTitle: 'jane eyre'},
]

const mappedResult = Object.entries(_.groupBy(queryResult, "userId"))
  .map(([k, v]) => ({
    userId: k,
    firstName: v[0].firstName,
    lastName: v[0].lastName,
    books: v.map(e => ({ bookId: e.bookId, bookTitle: e.bookTitle }))
}))

console.log(JSON.stringify(mappedResult, null, '   '))
```

## Question 8

Write a function that determines if a given argument is array-like, in the sense that it is iterable.

```js
isIterable(null); // false
isIterable('hello world'); // true
isIterable(document.querySelectorAll('.error-message')); // true
```

#### Ответ:

Можем ориентироваться на наличие ключа *[Symbol.iterator]*.
Значения *null* и *undefined* обрабатываем отдельно.

```js
function isIterable (obj) {
  if (obj === null || obj === undefined) {
    return false
  }
  return typeof obj[Symbol.iterator] === 'function'
}
```