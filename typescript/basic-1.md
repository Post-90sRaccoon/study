# 基本使用

#### 编译

* `tsc xxx.ts`

#### 解决编译后冲突

* `tsc --init`
* `tsconfig.json` `strict: false`

#### 自动编译

* `tsc --watch`

#### TS 有错误时不编译

* TS 有错但是不影响 JS 运行。

* `tsc --noEmitOnError`

#### 显示类型

```typescript
function greet(person: string, date: Date){ /**/ }

// TS 自动推断类型
let msg = 'abc'
msg = 1 //报错
```

#### 降级编译

* 通过修改 `tsconfig.json` 里的 `target` 为 `es5`

#### 严格模式

```json
"strict": true,
// 等价于下面两个
"noImplicitAny": true,
"strictNullChecks": true,
```

#### 常用类型

```json
"rootDir": "./src",
"outDir": "./dist",
```

```typescript
let arr: number[] = [1,2,3]
let arr: Array<number> = [1,2,3]
```

##### any：用于不希望某个特定值导致类型检查错误

```typescript
function greet(name: string): void {
  console.log("Hello," + name)
}

// TS 如果明确的知道匿名函数如何调用，可以推断出参数类型 上下文类型
const names = ['a','b','c']
names.forEach((s) => console.log(s.toUpperCase()))
```

#### 对象类型

```typescript
function printCoord(pt: { x: number, y: number }) {
   // 函数不指明返回类型会根据返回值自动推断
}

function printName(obj: { first: string, last?: string }) {
  // last 是可选的 不能传额外属性
  console.log(obj.last?.toUpperCase());
}
```

#### 联合类型

```typescript
function printId(id: number | string) {
  if (typeof id === 'string') {
     console.log(id.toUpperCase()) 
  }
}
```

#### 类型别名

```typescript
type Point = {
  x: number,
  y: number
}

function prointCoord(pt: Point) {}

type ID = number | string
```

#### 接口

```typescript
interface Point {
  x: number;
  y: number;
}

// 扩展接口
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean  
}

// 类型别名的拓展
type Animal = {
  name: string
}

type Bear = Animal & {
  honey: boolean
}

// 现有类型添加字段
interface MyWindow {
  count: number
}
interface MyWindow {
  title: string
}
```

#### 类型断言

```typescript
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas")

const x = 'hello' as number // 不可以
const x = ('hello' as unknown) 
```

