# JS-深拷贝
JS深拷贝
当我们在使用JS进行赋值时，进行简单类型的赋值，只会改变变量的值，而不会改变变量的地址，例如：
```
let a =10
let b = a
b = 20
```
此时改变b的值,a的值是不会收到影响的。
而当我们进行复杂类型赋值时，例如：
```
let a = {
  "name" : a,
  "age" : 18
}
let b = a 
b.name = "b"
```
此时我们b中的name值改变了，与此同时a中的name值也将变成b，大多数情况下我们都不希望这种情况发生，那么该怎么处理？
（ps：简单数据类型一般包括number,string,boolean，其他数据类型一般都为复杂数据类型）

处理这个问题涉及到JS中的深拷贝，JS的深拷贝会为新的变量重新申请一个新的地址块，不会指向原变量的地址，如何实现深拷贝，以下提供了两种方法。
#### 方法一：
```
let a = {
      "name" : a,
      "age" : 18
 }
let b = JSON.parse(JSON.stringify(a))
```
将a进行序列化之后赋值给b，这种方法虽然简单，但是有一定的缺陷，如果a对象中有函数，数组等复杂数据类型，在序列化之后将会丢失这些复杂数据类型，导致赋值不完全。

#### 方法二：
```
//深拷贝
let a = {
  "name" : a,
  "age" : 18,
  arr : [],
  fn : function(){
    console.log('fn')
  }
}

function deepCopy(obj){
  let newObj = Array.isArray(obj)?[]:{}//判断传过来的对象是对象还是数组
  for (let key in obj){
    if(obj.hasOwnProperty(key)){
       if(typeof(obj[key]) === Object){
        newObj[key] = deepCopy(obj[key])//如果循环到的属性本身还是对象，进行递归处理
        }else{
          newObj[key] = obj[key]
        }
    }
  }
  return newObj
}
let b = deepCopy(a)
```
