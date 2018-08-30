# Vue.js异步更新及nextTick

#### 举个例子

```vue
 <template>
  <div>
    <div>{{number}}</div>
    <div @click="handleClick">click</div>
  </div>
</template>

 export default {
    data () {
        return {
            number: 0
        };
    },
    methods: {
        handleClick () {
            for(let i = 0; i < 10000; i++) {
                this.number++;
            }
        }
    }
}
```


> **number会被遍历增加10000次，vue响应式系统中，vue会经历`setter -> Dep -> Watcher -> Patch -> 视图`，
这样dom被更新10000次。所有我们需要异步执行DOM更新。vue异步执行DOM更新。只要观察到数据变化，Vue将开启一个队列，并缓冲在同一时间循环中发生的所有的数据改变。
如果同一个watcher被多次触发，只会被推入到队列中一次。这种在缓冲时取出重复数据对于避免不惜要的计算和DOM操作上是非常的重要**

<hr/>

```javascript
setTimeout(function(){console.log(1)},0);

new Promise(function(resolve,reject){
   console.log(2);
   resolve();
}).then(function(){console.log(3)
}).then(function(){console.log(4)});

process.nextTick(function(){console.log(5)});

console.log(6);
//输出2,6,5,3,4,1
```