# 数组

!> JavaScript 中的数组是一种特殊的对象。它的工作原理与普通对象类似（以数字为属性名，但只能通过`[]` 来访问），但数组还有一个特殊的属性——`length`（长度）属性。这个属性的值通常比数组最大索引大 1。

## 创建数组

````javascript
// 传统方法
let a = new Array();
a[0] = "dog";
a[1] = "cat";
a[2] = "hen";
a.length; // 3

// 使用数组字面量（array literal）法更加方便：
let a = ["dog", "cat", "hen"];
a.length; // 3
````

* 注意，`Array.length` 并不总是等于数组中元素的个数，如下所示：
````javascript
var a = ["dog", "cat", "hen"];
a[100] = "fox";
a.length; // 101
````

* 记住：数组的长度是比数组最大索引值多一的数。

如果试图访问一个不存在的数组索引，会得到 `undefined`：

````javascript
typeof(a[90]); // undefined
````

## 属性

`Array.length`

Array 构造函数的 length 属性，其值为1（注意该属性为静态属性，不是数组实例的 length 属性）。

`get Array[@@species]`

返回 Array 构造函数。

`Array.prototype`

通过数组的原型对象可以为所有数组对象添加属性。

## 方法

`Array.from()`

从类数组对象或者可迭代对象中创建一个新的数组实例

`Array.isArray()`

用来判断某个变量是否是一个数组对象

`Array.of()`

根据一组参数来创建新的数组实例，支持任意的参数数量和类型

## 遍历数组

````javascript
// 可以通过如下方式遍历一个数组：

for (var i = 0; i < a.length; i++) {
    // Do something with a[i]
}

// ES2015 引入了更加简洁的 for...of 循环，可以用它来遍历可迭代对象，例如数组：

for (const currentValue of a) {
  // Do something with currentValue
}

// 遍历数组的另一种方法是使用 for...in 循环， 然而这并不是遍历数组元素而是数组的索引。
// 注意，如果哪个家伙直接向 Array.prototype 添加了新的属性，使用这样的循环这些属性也同样会被遍历。
// 所以并不推荐使用这种方法遍历数组：

for (var i in a) {
  // Do something with a[i]
}

// ECMAScript 5 增加了另一个遍历数组的方法，forEach()：

["dog", "cat", "hen"].forEach(function(currentValue, index, array) {
  // Do something with currentValue or array[index]
});
````

## 数组实例

所有数组实例都会从 `Array.prototype` 继承属性和方法。修改 `Array` 的原型会影响到所有的数组实例。

### 属性

`Array.prototype.constructor`

所有的数组实例都继承了这个属性，它的值就是 Array，表明了所有的数组都是由 Array 构造出来的。

`Array.prototype.length`

上面说了，因为 Array.prototype 也是个数组，所以它也有 length 属性，这个值为 0，因为它是个空数组。

### 方法

#### 修改器方法

下面的这些方法会改变调用它们的对象自身的值：

`Array.prototype.copyWithin()`

在数组内部，将一段元素序列拷贝到另一段元素序列上，覆盖原有的值。

`Array.prototype.fill()`

将数组中指定区间的所有元素的值，都替换成某个固定的值。

`Array.prototype.pop()`

删除数组的最后一个元素，并返回这个元素。

`Array.prototype.push()`

在数组的末尾增加一个或多个元素，并返回数组的新长度。

`Array.prototype.reverse()`

颠倒数组中元素的排列顺序，即原先的第一个变为最后一个，原先的最后一个变为第一个。

`Array.prototype.shift()`

删除数组的第一个元素，并返回这个元素。

`Array.prototype.sort()`

对数组元素进行排序，并返回当前数组。

`Array.prototype.splice()`

在任意的位置给数组添加或删除任意个元素。

`Array.prototype.unshift()`

在数组的开头增加一个或多个元素，并返回数组的新长度。

#### 访问方法

下面的这些方法绝对不会改变调用它们的对象的值，只会返回一个新的数组或者返回一个其它的期望值。

`Array.prototype.concat()`

返回一个由当前数组和其它若干个数组或者若干个非数组值组合而成的新数组。

`Array.prototype.includes()` 

判断当前数组是否包含某指定的值，如果是返回 true，否则返回 false。

`Array.prototype.join()`

连接所有数组元素组成一个字符串。

`Array.prototype.slice()`

抽取当前数组中的一段元素组合成一个新数组。

`Array.prototype.toSource()`

返回一个表示当前数组字面量的字符串。遮蔽了原型链上的 Object.prototype.toSource() 方法。

`Array.prototype.toString()`

返回一个由所有数组元素组合而成的字符串。遮蔽了原型链上的 Object.prototype.toString() 方法。

`Array.prototype.toLocaleString()`

返回一个由所有数组元素组合而成的本地化后的字符串。遮蔽了原型链上的 Object.prototype.toLocaleString() 方法。

`Array.prototype.indexOf()`

返回数组中第一个与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。

`Array.prototype.lastIndexOf()`

返回数组中最后一个（从右边数第一个）与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。

`Array.prototype.flat()`

flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

`Array.prototype.flatMap()`

flatMap() 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 和 深度值1的 flat 几乎相同，但 flatMap 通常在合并成一种方法的效率稍微高一些。这里我们拿map方法与flatMap方法做一个比较。

#### 迭代方法

在下面的众多遍历方法中，有很多方法都需要`指定一个回调函数作为参数`。在每一个数组元素都分别执行完回调函数之前，数组的`length`属性会被缓存在某个地方，所以，如果你在回调函数中为当前数组添加了新的元素，那么那些新添加的元素是不会被遍历到的。此外，如果在回调函数中对当前数组进行了其它修改，比如改变某个元素的值或者删掉某个元素，那么随后的遍历操作可能会受到未预期的影响。总之，不要尝试在遍历过程中对原数组进行任何修改，虽然规范对这样的操作进行了详细的定义，但为了可读性和可维护性，请不要这样做。

`Array.prototype.forEach()`

为数组中的每个元素执行一次回调函数。

`Array.prototype.entries()`

返回一个数组迭代器对象，该迭代器会包含所有数组元素的键值对。

`Array.prototype.every()`
如果数组中的每个元素都满足测试函
数，则返回 true，否则返回 false。

`Array.prototype.some()`

如果数组中至少有一个元素满足测试函数，则返回 true，否则返回 false。

`Array.prototype.filter()`

将所有在过滤函数中返回 true 的数组元素放进一个新数组中并返回。

`Array.prototype.find()`

找到第一个满足测试函数的元素并返回那个元素的值，如果找不到，则返回 undefined。

`Array.prototype.findIndex()`

找到第一个满足测试函数的元素并返回那个元素的索引，如果找不到，则返回 -1。

`Array.prototype.keys()`

返回一个数组迭代器对象，该迭代器会包含所有数组元素的键。

`Array.prototype.map()`

返回一个由回调函数的返回值组成的新数组。

`Array.prototype.reduce()`

从左到右为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。

`Array.prototype.reduceRight()`

从右到左为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。

`Array.prototype.values()`
返回一个数组迭代器对象，该迭代器会包
含所有数组元素的值。

`Array.prototype[@@iterator]()`

和上面的 values() 方法是同一个函数。