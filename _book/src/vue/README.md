# Vue

Vue.js是一款`mvvm`框架，核心思想是数据驱动视图，数据模型仅仅是普通的`javascript`对象。修改他们时，视图会更新。实现这些的核心就是`响应式系统`.


### 问答时间

#### 1、为什么使用Vue框架，在什么场景使用Vue框架？

答： 上万级数据需要瀑布流更新和搜索的时候，因为数据庞大的时候，用原生的dom操作JS和HTML都会有列表的HTML布局，迭代很困难
* 只需要用`v-for`在view层一个地方遍历数据即可，无需复制一段html代码在js和html两个地方。
* vue通过`Virtual Dom`就是在js中模拟DOM对象树来优化DOM操作

#### 2、聊聊你对Vue.js的template编译的理解？

答： 简而言之，就是先转化成AST树，再得到的render函数返回VNode（Vue的虚拟DOM节点）

首先，通过compile编译器把template编译成AST语法树（abstract syntax tree 即 源代码的抽象语法结构的树状表现形式），
compile是createCompiler的返回值，createCompiler是用以创建编译器的。另外compile还负责合并option。

然后，AST会经过generate（将AST语法树转化成render funtion字符串的过程）得到render函数，
render的返回值是VNode，VNode是Vue的虚拟DOM节点，里面有（标签名、子节点、文本等等）