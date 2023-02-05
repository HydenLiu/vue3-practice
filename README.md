# vue3 源码实践

* [虚拟DOM相关](./vdom/README.md)

### 三大个核心模块

- Reactivity Module
- Compiler Module
- Renderer Module
    - Render Phase (渲染阶段) : render function → virtual DOM node （返回虚拟DOM节点）
    - Mount Phase (挂载阶段): virtual DOM node → Webpage（创建Web页面）
    - Patch Phase (补丁阶段): 对比新旧vnode，更新页面
1. 首先，模版编译器将HTML转换为render function(渲染函数)（h函数）
2. 然后使用Reactivity Module（响应式模块）初始化响应式对象
3. 然后我们进入渲染阶段，它调用render函数，引用了响应式对象，观察这个响应式对象的变化，render函数返回一个虚拟DOM的节点（js对象）
4. 然后在挂载阶段，调用mount函数，使用这个虚拟DOM节点创建一个web页面
5. 最后，如果我们的响应式对象发生了变化，渲染器会再次调用render函数，创建一个新的虚拟DOM节点，然后新的和旧的虚拟DOM节点发送到patch function（补丁函数）中，然后根据需要更新我们的网页
