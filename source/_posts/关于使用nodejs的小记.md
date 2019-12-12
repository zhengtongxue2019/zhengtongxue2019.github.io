---
title: 关于使用nodejs的小记
date: 2019-11-26 10:32:34
tags:
---
## 以上是摘要
<!--more-->
## 以下是正文

## 初始化
`npm init` 初始化目录


## nodejs的模块导出`module.exports`
`module.exports`支持导出function函数，导出{}对象， 导出new后的实例。

* 导出的{}对象，可以被用来new实例化
* 导出的new后的实例，可以直接使用

module.exports 对象是由模块系统创建的。在写自己的模块时，需要在模块的最后写好模块接口，声明这个模块对外暴露什么内容，module.exports 就是提供了暴露接口的方法。

含有`module.exports`导出的文件是 CommonJS 模块；它可能会转换为 ES6 模块。

* `var calc = require("./demo.js").calc;` 对应的导出为 `exports.calc = calc;` 导出的是模块，使用是需要进一步指向具体导出的接口
* `var calc = require("./demo.js");` 对应的导出为 `module.exports = calc;` 导出的是模块中具体的接口，可以直接使用

* require: node 和 es6 都支持的引入
* export / import : 只有es6 支持的导出引入
```
export 和 export default
首先我们讲这两个导出，下面我们讲讲它们的区别

export与export default均可用于导出常量、函数、文件、模块等
在一个文件或模块中，export、import可以有多个，export default仅有一个
通过export方式导出，在导入时要加{ }，export default则不需要
export能直接导出变量表达式，export default不行。
```
* module.exports / exports: 只有 node 支持的导出



## 引入测试框架mocha

* 全局安装mocha`npm install -g mocha`

断言库有很多种，Mocha并不限制使用哪一种。

* 开发分支安装断言库`npm install --save-dev chai`
不使用`--save-dev`安装的话，`var expect = require("chai").expect;`会提示找不到`chai`。

### mocha基础使用

* 文件命名要求，测试`xx.js`需要对应测试文件`xx.test.js`
* 在`xx.test.js`文件中先引入断言库`var expect = require("chai").expect;`，断言功能由断言库来实现，Mocha本身不带断言库，所以必须先引入断言库。
* 编写单元测试用例
```
describe('描述当前分组的测试目的',function(){
    it("描述当前测试用例的测试目的", function(){
        expect(待测试动作的输出结果).to.be.equal(待测试动作的期望结果);
    });
});
```

使用实例参考 [测试框架 Mocha 实例教程](http://www.ruanyifeng.com/blog/2015/12/a-mocha-tutorial-of-examples.html)


* 断言的风格及使用

上面代码引入的断言库是chai，并且指定使用它的expect断言风格。

expect断言的优点是很接近自然语言，下面是一些例子。

```
// 相等或不相等
expect(4 + 5).to.be.equal(9);
expect(4 + 5).to.be.not.equal(10);
expect(foo).to.be.deep.equal({ bar: 'baz' });

// 布尔值为true
expect('everthing').to.be.ok;
expect(false).to.not.be.ok;

// typeof
expect('test').to.be.a('string');
expect({ foo: 'bar' }).to.be.an('object');
expect(foo).to.be.an.instanceof(Foo);

// include
expect([1,2,3]).to.include(2);
expect('foobar').to.contain('foo');
expect({ foo: 'bar', hello: 'universe' }).to.include.keys('foo');

// empty
expect([]).to.be.empty;
expect('').to.be.empty;
expect({}).to.be.empty;

// match
expect('foobar').to.match(/^foo/);
```

基本上，expect断言的写法都是一样的。头部是expect方法，尾部是断言方法，比如equal、a/an、ok、match等。两者之间使用to或to.be连接。
