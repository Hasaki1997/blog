## new
>`new` 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

当代码 `new Foo(...)` 执行时，会发生以下事情：
1. 一个继承自 `Foo.prototype` 的新对象被创建。(关于原型链=> [原型链](./原型链.md))
1. 使用指定的参数调用构造函数 `Foo` ，并将 `this` 绑定到新创建的对象。`new Foo` 等同于 `new Foo()`，也就是没有指定参数列表，`Foo` 不带任何参数调用的情况。
1. 由构造函数返回的对象就是 `new` 表达式的结果。如果构造函数没有显式返回一个对象，则使用步骤1创建的对象。（一般情况下，构造函数不返回值，但是用户可以选择主动返回对象，来覆盖正常的对象创建步骤）

简单说就是以下步骤
 	 1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。
   2、属性和方法被加入到 this 引用的对象中。
 	 3、新创建的对象由 this 所引用，并且最后隐式的返回 this 。
```js
 var obj  = {};
 obj.__proto__ = Base.prototype;
 Base.call(obj);
```


-----
新增一个 new 的实现
```js
function znew() {
  const obj = new Object();
  const fn = [].shift.call(arguments);
  obj.__proto__ = fn.prototype;
  const ret = fn.apply(obj, arguments);
  return (typeof ret === 'object') ? ret : obj;
};
```