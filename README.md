### 微前端核心

微前端通过技术手段隔离js、css等资源文件，统一一个标准，将主应用统一管理微应用，来实现微应用的独立性和统一性。

### why not iframe

1. ui问题的统一，弹窗的统一
2. url路径的状态统一，spa应用中的iframe是无法影响你的url的
3. 主应用的预加载
4. 复杂业务场景下的iframe的变量传递麻烦 

### 沙箱分类

1. snapshotSandbox快照沙箱： 通过浅拷贝一份window对象，然后diff微应用内的变化，如果更新，那么覆盖。
    - 问题
      - 存在很多次diff可能，影响性能
2. LegacySandbox遗留沙箱： 通过历史修改的主应用的记录，来修复
    - 问题
      - 存在主应用的同一路由下存在不同的微应用，存在window不同的变量名冲突问题
3. ProxySandbox代理沙箱： 通过proxy代理，将不同的微应用变量限制在微应用的fakeWindow内来解决冲突

### 沙箱隔离原理

1. 把要执行 JS 代码放在一个立即执行函数中，且函数入参有 window, self, globalThis
2. 给这个函数 绑定上下文 window.proxy
3. 执行这个函数，并 把上面提到的沙箱对象 window.proxy 作为入参分别传入

