# vue与其他框架对比

### React

#### 相同点

* 数据驱动视图，提供响应式的视图组件
* 都有Virtual DOM，组件化开发，通过props参数进行父子组件数据的传递，都实现webComponents规范
* 数据流动单向
* 都支持服务端渲染
* 都有支持native的方案，React的React native，Vue的weex


#### 不同点

* 数据绑定：Vue有实现了双向数据绑定，React数据流动是单向的
* 不需要`shouldComponentUpdate`