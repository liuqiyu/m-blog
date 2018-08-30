# vuex
单项数据流。
Vuex可以被看作项目中所有组件的数据中心,我们将所有组件中共享的State抽离出来,任何组件都可以访问和操作我们的数据中心.

![Image text](images/vuex.png)

**流程：形成一个闭环** 

component -> (`dispatch`) -> Actions -> (`commit`) -> Mutations -> (`Mutate`) -> State -> (`render`) -> component

