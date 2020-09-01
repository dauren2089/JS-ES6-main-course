## 1. Отличие CONST, VAR и LET
# VAR уже не используется 
# у VAR область видимости ограничена функцией

# Пример с использованием VAR
```js
for (var i = 0; i < 3; i++) {
	setTimeout(function () {
		console.log(i)
	}, i*100);
}
```
# при выводе получаем
# 3
# 3
# 3
# ответ кроется в в том что при использовании VAR, система создает ячейку памяти для VAR заранее и только один раз.
# цикл завершается до того как мы выведем значение i, соответственно после таймаута, мы ссылаемся на ячейку со значением "3".

# Пример с использованием LET
```js
for (let i = 0; i < 3; i++) {
	setTimeout(function () {
		console.log(i)
	}, i*100);
}
```
# Вывод: 1, 2 3
# область видимости LET ограничивается в запускаемым блоком.
# при каждой итерации LET создает новую переменную.


## 2. Arrow-функции
# - легковесные версии обычных функции
# - сохраняют лексическо значение this
# и очень удобны передачи в качестве аргумента другим функциям
# - удобны для регистрации как обработчики событий (EVENTLISTENERS)
# - нет свойства PROTOTYPE, не могут выступать в качестве конструкторов
# и не могут быть вызваны с "new".

# пример традиционной функции
```js
function square(x) {
	return x*x;
}
```
# пример стрелочной функции
```js
const square = (x) => x*x;
```
#традиционной функции для преобразование массива:
```js
const res = arr
	.map(function(el) {
		return parseInt(el);
	})
	.filter(function(num) {
		return num%2;
	})
	.reduce(function(max, value) {
		return Math.max(max, value);
	}, 0);

console.log(res);
```

#стрелочная функция для преобразование массива:
```js
const arr = ['1', '2', '3', '4'];
```
# преобразуем со строки в числа
# выводим только четные числа
# находим максимальное число в четных числах
```js
const res = arr
	.map((el) => parseInt(el))
	.filter((num) => num%2)
	.reduce((max, value) => Math.max(max, value), 0);

conlose.log(res);
```
# на выходе получаем '3'

# Преимущество arrow функции перед традиционной функцией
# Пример функции с this
```js
const greeter = {

	greet: function (name) {
		console.log('Hello ', name);
	},

	greetAll: function (names) {
		names.forEach(function(name){
			this.greet(name);
		# выведет ошибку!
		# this ссылается на внешнюю функцию!
		});
	}
};

greeter.greetAll(['Bob', 'Mark', 'Pete']);
```
# Пример arrow функции с this
# выведет приветствие и список имен
```js
const greeter = {

	greet: function (name) {
		console.log('Hello ', name);
	},

	greetAll: function (names) {
		names.forEach((name) => {
			this.greet(name);
		# так как использовалась стрелочная функция
		# this ссылается на необходимый параметр
		});
	}
};

greeter.greetAll(['Bob', 'Mark', 'Pete']);
```
## 3. Параметры по-умолчанию
# (DEFAULTS PARAMETERS)
```js
function f(a = 10, b = 20) {}
```
# - Устанавливаются если не передать значение (или передать UNDEFINED)
# - Чаще идут последними в списке
# - Могут иметь лиюой тип

# Пример функции с параметром по умолчанию (START = 0)
```js
function fetchOrders(count, start = 0) {
	console.log('Getting', count,
		'orders starting from', start);
}

fetchOrders(15);
```
## 4. REST parameter
```js
function f(a, b, ...others) {}
```
# - используй если неуверен в количестве параметров функции
# - REST параметр должен быть последним параметром!
# - максимум ОДИН REST параметр в функции!

#Пример: старый метод использования псевдомассивов когда нет точного количества аргуметов
```js
max(1, 3);
max(1, 3, 3, 4, 5);

function max() {
	# можно использовать только через псевдо-массив Arguments
	var numbers = Array.prototype.slice.call(arguments)
}
```
#numbers = [1, 3]

# Пример использования REST параметра
```js
max(1, 3);
max(1, 3, 4, 5, 6, 7);

const max = (a, b, ... numbers) => {
	console.log(a, b, numbers)
}
```
# a = 1, b = 3, numbers = []
# a = 1, b = 3, numbers = [4, 5, 6, 7]

## 5. SPREAD operator
# - похож на REST, но действует в обратном направлении
# - раскладывает массив для функции или для других массивов
# - разворачивает массив, превращая его в список аргументов

# пример, находим максимальное число
```js
const arr = [1, 2, 3];
```
# можно было сравнить элементы массива методом apply() для поиска Max
```js
const res = Math.max.apply(Math, arr);
console.log(res);
```
# вывод ~ 3

# можно по проще > используя SPREAD оператор для поиска Max
```js
const res = Math.max(...arr);
console.log(res);
```
# вывод ~ 3

# еще один пример, сравнение 2х массивов + добавление других аргументов
```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6, 7];
const res = Math.max(8, ...arr1, 10, ...arr2, 9);
console.log(res)
```
# вывод ~ 10

## 6. Деструктуризация объектов
# (Object destructuring)
# - упрощает получение свойств из объектов
# - поддерживает вложенность и значения по умолчанию
# - позволяет работать с параметрами йункции
# - поддерживает REST-элементы
# - позволяет лаконично достать объекты из структуры данных
# - позволяет указать объекты по-умолчанию

#  ПРИМЕРЫ
```js
const person = {
	firstName: 'Peter',
	lastName: 'Smith',
	age: 27
};
```
# Пример извлечения объекта традиционно, путем обращения через сам объект
```js
const firstName = person.firstName;
const lastName = person.lastName;
```
# пример деструктуризации объекта
```js
const { firstName, lastName } = person;

console.log(firstName, lastName);
```
# пример деструктуризации сложного объекта
```js
const person = {
	name: {
		first: 'Peter',
		last: 'Smith'
	},
		age: 27
};

const { name: { first: firstName, last: lastName } } = person;

console.log(firstName, lastName);
```
# Присвоение значения по умолчанию
```js
const {role = 'user'} = person;
console.log(role); 
```
# user;

# доставь значение permissions,но если его нет в массиве, то достаноь из объекта по умльчанию ссылаясь на пустого объекта в ROLE
```js
const { permissions: { role = 'user' } = {} } = person;
console.log(role);
```
#user;

## 7. Деструктуризация объектов параметров функции
# пример вызова функции с аргументами + по-умолчанию
```js
function connect({
	host = 'localhost',
	port = 21,
	user = 'guest' }) {

	console.log('user:', user);
}

connect({port: 1111});
```
#пример вызова функции без аргументов (параметров)
# значения присваиваются по-умолчанию из указанных выше значений
```js
function connect({
	host = 'localhost',
	port = 21,
	user = 'guest' } = {}) {
	// {} << ссылаемся на пустой объект, указвая,
	// что необходимо деструкрировать объект и взять с указанных параметров по-умолчанию
	console.log('user:', user);
}
```
# при вызове функции аргументы могут не указываться
```js
connect();
```
# Пример деструктуризации + использования REST элементов
```js
const dict = {
	duck: 'quack',
	dog: 'wuff',
	mouse: 'squeak'
};
```
# Деструктурируем значение DUCK и использум REST для вывода оставшихся элементов
```js
const { duck, ...otherAnimals } = dict;

console.log(duck, otherAnimals);
```
## 8. Деструктуризация массивов
# - поддерживает все те же возможности, что и объекты
# - можно пропускать значения
# - можно использовать синтаксис деструктуризации для массивов и 
# объектов в одном выражении

# пример Деструктуризации массивов
```js
const fib = [1, 2, 3, 4, 5, 6, 7];
```
# создаем новые переменные используя Деструктуризацию
```js
const [a, b, c] = fib;

console.log(a, b, c);

```
# вывод: a = 1; b = 2; c = 3;

# пропускаем элементы массивов путем добавления " , "
```js
const [, a, , b] = fib;

console.log(a, b);
```
#вывод: а = 2; b = 4;
```js
const line = [[10, 17], [14, 7]];
```

# элементы по умолчанию
# деструктуризация массивов
```js
const dict = {
	duck: 'quack',
	dog: 'wuff',
	mouse: 'squeak',
	hamster: "squeak"
};
```
#вариант №1
```js
const res = Object.entries(dict).filter((arr) => arr[1] === 'squeak');

console.log(res);
```
#вывод

#вариант №2
```js
const res = Object.entries(dict)
	.filter(([, value]) => value === 'squeak');
	.map(([key]) => key);

console.log(res);
```
# вывод:

```js
const shape = {
	type: 'segment',
	coordinates: {
		start: [10, 15],
		end: [17, 15]
	}
};

const { coordinates:
	{ start: [startX, startY],
		end: [endX, endY]}} = shape;

console.log(startX, startY, endX, endY);
```
# вывод:

## 9. ШАБЛОННЫЕ СТРОКИ (TEMPLATE LITERALS)
# - Поддерживает выражения, вызовы функций
# - Поддерживают перенос строки
# - Результат - обычная строка (а не новый тип)

# Пример без шаблонных строк, а использованием конкатенации строк
```js
const user = 'Bob';
const num = 17;
const txt = 'Hello, ' + user + ' you have ' + num 
	+ ' letters in your inbox';
```
#Пример использованием шаблонных строк
```js
const txt = `Hello ${user} you have ${num} letters in your inbox`;
```
# вызов функции в шаблонной строке
```js
const date = `Now is ${Date.now()}`;

console.log(txt, date); 
```
# вставка тэгов через шаблонные строки
```js
const items = ['tea', 'coffee'];

const templateHtml = `
	<ul>
		<li>${items[0]}<\li>
		<li>${items[1]}<\li>
	<\ul>
`;
```

## 10. ОБЪЕКТЫ в ECMA2015
```js
> const a = {x, y}
> const a = { sayHi() {...}}
> const a = { [dynamickey]: value"}

> const res = Object.assign(dest, scr1, src2)
```
#объекты в старых версиях
```js
const point = {
	x: x,
	y: y,

	draw: function(parameter) {
		draw++;
	}
};
```
## 11. объекты в ECMA 2015
```js
const p = {
	x, y,

	draw(parameter) {
		draw++;
	}
}
```
# перезапись свойств объектов из двух массивов
# объект со свойствами заданный по-умолчанию
```js
const defaults = {
	host: 'localhost',
	dbName: 'blog',
	user: 'admin'
};
```
# объект со свойствами заданный пользователем
```js
const opts = {
	user: 'john',
	password: 'password'
};
```
# объединение и перезапись свойств через метод Object.assign
```js
const res =Object.assign({}, defaults, opts);
console.log(res);
```
# вывод:
```JSON
{
	host: 'localhost',
	dbName: 'blog',
	user: 'john',
	password: 'password'
}
```
## 12. SPREAD оператор
# - Разворачивает объект, превращая его в список свойтв
# - Можно комбинировать с любым другим синтаксисом создания объектов

# ПРИМЕР: перезапись свойств объектов из двух массивов
# объект со свойствами заданный по-умолчанию
```js
const defaults = {
	host: 'localhost',
	dbName: 'blog',
	user: 'admin'
};
```
# объект со свойствами заданный пользователем
```js
const opts = {
	user: 'john',
	password: 'password'
};
const port = 8080;
```
# объединение и перезапись свойств через SPREAD оператор
```js
const res = { ...defaults, ...opts, port, connect() };

console.log(res);
```
# вывод:
```JSON
{
	host: 'localhost',
	dbName: 'blog',
	user: 'john',
	password: 'password',
	port: 8080
}
```
## 13. PROTOTYPE
# Хранят общие методы объектов
# 3 способа связать "объект-прототип"

# 1. Object.setPrototypeOf(obj, proto)
# dog и cat делегируют функцию say с прототипа ANIMAL
```js
const animal = {
	say: function(){
	console.log(this.name, 'goes', this.voice);
	}
}

const dog = {
	name: 'dog',
	voice: 'woof'
};
```
# создаем связь с ANIMAL и dog
```js
Object.setPrototypeOf(dog, animal);

const cat = {
	name: 'cat',
	voice: 'meow'
};
```
# создаем связь с ANIMAL и cat
```js
Object.setPrototypeOf(cat, animal);

dog.say();
cat.say();
```
# 2.1 const obj = Object.create(proto)
```js
const Animal = {
	say: function(){
	console.log(this.name, 'goes', this.voice);
	}
}
```
# cоздавем пустой объект через метод Object.create()
```js
const dog = Object.create(animal);
```
# и затем присваиваем свойства
```js
dog.name = 'Dog';
dog.voice = 'woof';

dog.say();
```
# 2.2 выносим в отдельную функцию >> ФУНКЦИЯ КОНСТРУКТОР
 
```js
function createAnimal(name, voice) {
	const res =Object.create(animal);
	res.name = name;
	res.voice = voice;

	return res;
}

const dog = createAnimal('dog', 'woof');
dog.say();
```
# 3. const obj = new Obj()
```js
function Animal(name, voice) {
	this.name = name;
	this.voice = voice;
}

Animal.prototype.say = function() {
	console.log(this.name, 'goes', this.voice);
};
```
# создаем новые объекты через new prototype
```js
const dog = new Animal('Dog', 'woof');
const cat = new Animal('Cat', 'meow');

dog.say();
cat.say();
```
## 14. Cвойства классов
# Инициалозация полей в теле класса
# функции в теле класса - привязаны к объекту
# Статичные поля
# Статичные методы
```js
class Counter {
	count = 0;

	increment = () => {
		this.count += Counter.incrementStep;
	}

	static incrementStep = 2;

	static incrementAll = function (arr) {
		arr.forEach((c) => c.increment());
	}
}
```
## 15. Javascript Модули
```js
import { a, b as c} from './file';
import X from './file';
import * as lib from 'file';

export {
	a, b , c as d
}
export default Z;
```
# Примитивный спосою экспорта и импорта функции.
# 1. добавляем в файле команду export
```js
export {
	add, subtract, multiply, divide
};
```
# 2. добавляем команду import в где необходимо использовать экспортируемые функции

```js
import {add, subtract as sub, multiply as mult } from './mymath';
```
