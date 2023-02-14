# vue3 源码实践

## 三大个核心模块

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

## Virtual DOM（虚拟DOM）

1. 让组件的渲染逻辑完全从真实的DOM解耦，并让它更直接的重用框架的运行时在其他环境（不仅仅是在web端，也包括iOS 和 Android等原生环境），可以创建自定义渲染的解决方案
2. 以编程的方式构造、检查、克隆以及操作所需要的DOM结构，在实际返回渲染引擎之前，你可以利用javascript的全部能力进行操作，dom渲染的开销 > js运行的开销，优化渲染性能

## Diff算法
### 简单的Diff算法
1. 遍历新旧两组子节点中数量较少的那一组，并逐个调用patch函数进行打补丁，然后比较新旧两组子节点的数量，如果新的一组子节点数量更多，说明有新子节点需要挂载；否则说明在旧的一组节点中，有节点需要卸载。
2. 拿新的一组子节点中的节点去旧的一组子节点中寻找可复用的节点。如果找到了，则记录该节点的位置索引。我们把这个位置的索引称为最大索引。在整个更新过程中，如果一个节点的索引值小于最大索引，则说明该节点对应的真实节点DOM需要移动。

### 双端Diff算法
* 在新旧两组子节点的四端之间分别进行比较，试图找到可复用的节点

### 快速Diff算法
* 先处理新旧两组子节点中相同的前置节点和相同的后置节点。当前置节点和后置节点全部处理完毕后，如果无法简单的通过挂载新节点或者卸载已经不存在的节点来完成更新，则需要根据节点的索引关系，来构造出一个最长递增子序列。最长递增子序列所指向的节点即为不需要移动的节点。
* vue3 diff算法里的[最长递增子序列](https://github.com/vuejs/core/blob/main/packages/runtime-core/src/renderer.ts#L2407-L2446)
