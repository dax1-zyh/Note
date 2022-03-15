  ## tsconfig.json
  tsconfig.json是ts编译器的配置文件，ts编译器可以根据它的信息来对代码进行编译

## 编译选项

- **"include"** 用来指定哪些ts文件需要被编译，**表示任意目录，*表示任意文件

```typescript
  "include": [
    "./src/**/*"
  ],
```
- **"exclude"** 不需要被编译的文件目录

```typescript
  "exclude": [
    "node_modules"
  ]
```
- **“extends”** 定义被继承的配置文件 
- **“files”** 指定被编译文件的列表，只有需要编译的文件少时才会用到
- **“compilerOptions”** 编译器的选项（**重点**，以下为compilerOptions的子选项）
- **“target”**  指定被编译为的ES版本 `"target":"ES2015"`
- **“module”**  指定要使用的模块化的规范 `"module":"ES2015"` 
- **“lib”**  指定项目中要使用的库（一般在浏览器环境中不配置此项，一般用于Node环境）
- **“outDir”**  指定编译后文件所在的目录 `"outDir":"./dist"` 
- **“outFile”**   配置后所有全局作用域中的代码会合并到同一个文件中 `"outDir":"./dist/api.js"` 
- **“allowJs”**  是否对JS文件进行编译，默认是false
- **“checkJs”**  是否检查JS代码是否符合语法规范，默认是false
- **“removeComments”**  是否移除注释，默认是false
- **“noEmit”**  不生成编译后的文件，默认false
- **“noEmitOnError”**  当编译有错误时，不生成编译后的文件，默认false
- **“strict”**  所有严格模式的总开关，默认false（配置后相当于以下配置全都开启）
- **“alwaysStrict”**  用来设置编译后的文件是否使用严格模式，默认false
- **“noImplicitAny”**  不允许隐式的any类型，默认false
- **“noImplicitThis”**  不允许不明确类型的this，默认false
- **“strictNullChecks”**  严格地检查空值，默认false

## 官方文档
更多编译选项参考 [TS中文网：编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)