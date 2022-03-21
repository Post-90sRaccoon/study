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
