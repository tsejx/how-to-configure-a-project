# TypeScript

TypeScript 相关配置文件：

- `tsconfig.json`
- `tslint.json`
- `tslint.yml`

## 项目配置文件

TypeScript 使用 `tsconfig.json` 作为其配置文件，它主要包含两块内容：

1. 指定待编译的文件
2. 定义编译选项

另外，一般来说，`tsconfig.json` 文件所处的路径就是当前 TypeScript 项目的根路径。

### 配置说明

- `compilerOptions`：配置编译选项
- `files`：指定编译文件

这里的待编译文件是指入口文件，任何被入口文件依赖的文件，比如 `foo.ts` 依赖 `bar.ts`，那这里并不需要写上 `bar.ts` ，编译器会自动把所有的依赖文件纳为编译对象。

也可以使用 `include` 和 `exclude` 来指定和排除待编译文件：

```json
{
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

#### 文件指定

`files` 属性是一个数组，数组元素可以是相对文件路径和绝对文件路径。

`include` 和 `exclude` 属性也是一个数组，但数组元素是类似 `glob` 的文件模式。它支持的 `glob` 通配符包括：

* `*`：匹配 0 或多个字符（注意：不含路径分隔符）
* `?`：匹配任意单个字符（注意：不含路径分隔符）
* `**/`：递归匹配任何子路径

在编译器眼里，TypeScript 文件指拓展名为 `.ts`、`.tsx` 或 `.d.ts` 的文件。

如果开启了 `allowJs` 选项，那么 `.js` 和 `.jsx` 文件也属于 TypeScript 文件。

如果仅仅包含一个 `*` 或 `.*`，那么只有 TS 文件才会被包含。

如果 `files` 和 `include` 都未设置，那么除了 `exclude` 排除的文件，编译器会默认包含路径下的所有 TS 文件。

如果未设置 `exclude`，那其默认值为 `node_modules`、`bower_componentss`、`jspm_packages` 和 编译选项 `outDir` 指定的路径。

`exclude` 只对 `include` 有效，对 `files` 无效。即 `files` 指定的文件如果同时被 `exclude` 排除，那么该文件仍然会被编译器引入。

面提到，任何被 `files` 或 `include` 引入的文件的依赖会被自动引入。
反过来，如果 `B.ts` 被 `A.ts` 依赖，那么 `B.ts` 不能被 `exclude` 排除，除非 `A.ts` 也被排除了。

有一点要注意的是，编译器不会引入疑似为输出的文件。比如，如果引入的文件中包含 `index.ts`，那么 `index.d.ts` 和 `index.js` 就会被排除。通常来说，只有拓展名不一样的文件命名法是不推荐的。

`tsconfig.json` 也可以为空文件，这种情况下会使用默认的编译选项来编译所有默认引入的文件。

#### 编译选项

| 选项字段                 | 说明                                                         | 类型       | 默认值                                                       |
| ------------------------ | ------------------------------------------------------------ | ---------- | ------------------------------------------------------------ |
| `allowJs`                | 允许编译 JS 文件                                             | `boolean`  | `false`                                                      |
| `checkJs`                | 报告 JS 文件中存在的类型错误需要配合 `allowJs` 使用          | `boolean`  | `false`                                                      |
| `declaration`            | 生成对应的 `.d.ts` 文件                                      | `boolean`  | `false`                                                      |
| `declarationDir`         | 生成的 `.d.ts` 文件存放路径默认与 `.ts` 文件相同             | `string`   | `-`                                                          |
| `experimentalDecorators` | 启用实验功能-ES 装饰器                                       | `boolean`  | `false`                                                      |
| `jsx`                    | 在 `.tsx` 中支持 JSX ：`React` 或 `Preserve` ，[详细说明](http://www.typescriptlang.org/docs/handbook/jsx.html) | `string`   | `PreServer`                                                  |
| `jsxFactory`             | `jsx` 设置为 `React` 时使用的创建函数                        | `string`   | `React.createElment`                                         |
| `lib`                    | 编译时引入的 ES 功能库，包括：`es5` 、`es6`、`es7`、`dom` 等。如果未设置，则默认为： `target` 为 `es5` 时: `["dom", "es5", "scripthost"]` `target` 为 `es6` 时: `["dom", "es6", "dom.iterable", "scripthost"]` | `string[]` | `-`                                                          |
| `module`                 | 生成的模块形式：`none`、`commonjs`、`amd`、`system`、`umd`、`es6`、`es2015` 或 `esnext` 只有 `amd` 和 `system` 能和 `outFile` 一起使用 `target` 为 `es5` 或更低时可用 `es6` 和 `es2015` | `sting`    | `target === "es3" or "es5" ? "commonjs" : "es6"`             |
| `moduleResolution`       | 模块解析方式，[详细说明](http://www.typescriptlang.org/docs/handbook/module-resolution.html) | `string`   | `module === "amd" or "system" or "es6" ? "classic" : "node"` |
| `noImplicitAny`          | 存在隐式 `any` 时抛错                                        | `boolean`  | `false`                                                      |
| `noImplictReturns`       | 不存在 `return` 时抛错                                       | `boolean`  | `false`                                                      |
| `noImplicitThis`         | `this` 可能为 `any` 时抛错                                   | `boolean`  | `false`                                                      |
| `outDir`                 | 编译生成的文件存放路径默认与 `.ts` 文件相同                  | `string`   | `-`                                                          |
| `sourceMap`              | 生成 `.map` 文件                                             | `boolean`  | `false`                                                      |
| `target`                 | 生成 `.js` 文件版本                                          | `string`   | `es3`                                                        |

#### 类型相关

类型相关的选项包括 `typeRoots` 和 `types`。

有一个普遍的误解，以为这两个选项适用于所有的类型声明文件，包括用户自定义的声明文件。其实不然。
这两个选项只对通过 npm 安装的声明模块有效，用户自定义的类型声明文件与它们没有任何关系。

声明模块通常会包含一个 `index.d.ts` 文件，或者其 `package.json` 设置了 `types` 字段。

默认的，所有位于 `node_modules/@types` 路径下的模块都会引入到编译器。
具体来说是，`./node_modules/@types`、`../node_modules/@types`、`../../node_modules/@types` 等等。

`typeRoots` 用来指定默认的类型声明文件查找路径，默认为 `node_modules/@types` 。比如:

```json
{
  "compilerOptions": {
    "typeRoots": ["./typings"]
  }
}
```

上面的配置会自动引入 `./typings` 下的所有 TS 类型声明模块，而不是 `./node_modules/@types` 下的模块。

如果不希望自动引入 typeRoots 指定路径下的所有声明模块，那可以使用 types 指定自动引入哪些模块。比如：

```json
{
  "compilerOptions": {
    "types" : ["node", "lodash", "express"]
  }
}
```

只会引入 `node` 、 `lodash` 和 `express` 三个声明模块，其它的声明模块则不会被自动引入。
如果 `types` 被设置为 `[]` ，那么将不会自动引入任何声明模块。此时，如果想使用声明模块，只能在代码中手动引入了。

请记住，自动引入只对包含全局声明的模块有效。比如 jQuery ，我们不用手动 `import` 或者 `///<reference/>` 即可在任何文件中使用 `$` 的类型。再比如，对于 `import 'foo'` ，编译器会分别在 `node_modules` 和 `node_modules/@types` 文件下查找 `foo` 模块和声明模块。

基于此，如果想让自定义声明的类型不需要手动引入就可以在任何地方使用，可以将其声明为全局声明 `global` ，然后让 `files` 或者 `include` 包含即可。
比如：

```ts
declare global {
  const graphql: (query: TemplateStringsArray) => void;
  namespace Gatsby {
    interface ComponentProps {
      children: () => React.ReactNode,
      data: RootQueryType
    }
  }
}
```

这样的话，就可以在任何地方直接使用 graphql 和 Gatsby 对应的类型了。

配置复用
可以使用 `extends` 来实现配置复用，即一个配置文件可以继承另一个文件的配置属性。

比如，建立一个基础的配置文件 configs/base.json ：

```json
{
  "compilerOptions": {
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

然后，`tsconfig.json` 就可以引用这个文件的配置了：

```json
{
  "extends": "./configs/base",
  "files": [
    "main.ts",
    "supplemental.ts"
  ]
}
```

这种继承有两种特点：

* 继承者中的同名配置会覆盖被继承者
* 所有相对路径都被解析为其所在文件的路径

## 代码规范和错误检查工具
