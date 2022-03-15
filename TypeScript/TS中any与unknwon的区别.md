## 主要区别
unknown和any都是TS中的顶级类型，但主要区别在于：使用any相当于彻底放弃了类型检查，而unknown类型相较于any更加严格，在执行大多数操作之前，会进行某种形式的检查。


## 赋值操作
```typescript
let foo: any = 123;
console.log(foo.msg); // 符合TS的语法
let a_value1: unknown = foo;   // OK
let a_value2: any = foo;      // OK
let a_value3: string = foo;   // OK


let bar: unknown = 222; // OK 
console.log(bar.msg); // Error
let k_value1: unknown = bar;   // OK
let K_value2: any = bar;      // OK
let K_value3: string = bar;   // Error
```
- foo是any类型，任何操作都是没有类型检查的，因此对其进行任意类型的赋值都是合法的
- bar是unknown类型，因此不能确定是否有msg属性，不能通过语法检查，同时unknown类型的值也不能赋值给any和unknown以外的类型变量

## 联合类型
在联合类型中,unknown类型会吸收任何类型。即如果任一组成类型是unknown，联合类型也会相当于unknown

```typescript
type UnionType1 = unknown | null;       // unknown
type UnionType2 = unknown | undefined;  // unknown
type UnionType3 = unknown | string;     // unknown
type UnionType4 = unknown | number[];   // unknown
```

而any更是能吸收unknown类型，如果任一组成类型是any，则联合类型相当于any

```typescript
type UnionType5 = unknown | any;  // any
```

## 交叉类型

```typescript
type IntersectionType1 = unknown & null;       // null
type IntersectionType2 = unknown & undefined;  // undefined
type IntersectionType3 = unknown & string;     // string
type IntersectionType4 = unknown & number[];   // number[]
type IntersectionType5 = unknown & any;        // any
```
由于每种类型都可以赋值给unknown类型，所以在交叉类型中包含unknown不会改变结果。