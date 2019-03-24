# Kibana源码涉及的重点内容
- nodejs 核心库
    - process
    - Array
    - Object
    - crypto
    - 
- nodejs三方库
    - lodash
    - angular
    - commander
    - bluebird
    - hapi
    - 
    - 
- 命令行 模块
- kibana 模块
- discover 模块   (限开发者使用)
- visualize 模块
- Dashboard 模块  
- Timelion 模块   (限开发者使用或者去掉此模块)
- Console 模块    (限开发者使用)
- Management 模块 (限开发者使用)
- ES 6
    - Promise
    - Symbol

## bat语法    kibana.bat

## js严格模式

## Lodash 
工具库，内部封装了诸多对字符串、数组、对象等常见数据类型的处理函数
```js
_.find    //按条件查找
_.intersection    //找出共同元素
_.wrap(value, [wrapper=identity]) //创建一个函数。提供的 value 包装在 wrapper 函数的第一个参数里。 任何附加的参数都提供给 wrapper 函数。 被调用时 this 绑定在创建的函数上
_.wrap.partial(func, [partials]) //创建一个函数。 该函数调用 func，并传入预设的 partials 参数
_.set(object, path, value)  //设置 object对象中对应 path 属性路径上的值，如果path不存在，则创建
_.get(object, path, [defaultValue]) // 根据 object对象的path路径获取值。 如果解析 value 是 undefined 会以 defaultValue 取代。
_.has(object, path) //检查 path 是否是object对象的直接属性
_.merge(object, [sources])
_.compact(array)  // 创建一个新数组，包含原数组中所有的非假值元素。例如false, null, 0, "", undefined, 和 NaN 都是被认为是“假值”
_.constant(value) // 创建一个返回 value 的函数
_.flatten(array) // 减少一级array嵌套深度
_.assign(object, [sources]) //分配来源对象的可枚举属性到目标对象上。 来源对象的应用规则是从左到右，随后的下一个对象的属性会覆盖上一个对象的属性。 
_.pick(object, [props]) // 创建一个从 object 中选中的属性的对象。
_.defaults(object, [sources])
_.omit(object, [props]) // 反向版 _.pick

```
## 全局对象process
```js
process.env()
process.exit(n)
```

## commander
```js
Command(name) //初始化一个命令
program.version(s).description(s)
program.command(s).description(s).action(fn).option(s,s)
program.error(s)
program.command(s, null, { noHelp: true }).action(fn)
program.parse(argv) 
```

## bluebird

## Array
splice()
shift()

## _asyncToGenerator

## Promise
```js
Promise.resolve(value) //返回一个以给定值解析后的Promise对象
Promise.reject(reason) // 返回一个用reason拒绝的Promise
```

## bluebird
fromNode

## ECMAScript 6
### Symbol
iterator

### Object
```js
Object.assign
Object.create() //方法会使用指定的原型对象及其属性去创建一个新的对象
```
### crypto
createHash
digest

### hapi
```js
server.decorate(type, property, method, [options])
server.ext(event, method, [options])
```
## angular
angular.module


## items
- AngularJs 渲染的逻辑
- module的逻辑
- 视图module的逻辑

## 可能要完成的工作
- 将visualize剥离形成单独的项目，但visualize来源于剥离前的项目
- 修改visualize项目视图
- 针对特定的内容开发visualize插件
- 或者进行前端权限过滤
- 用户和安全模块，需借鉴x-pack

## 
#### 搭建dev环境
- https://github.com/elastic/kibana/blob/master/CONTRIBUTING.md
