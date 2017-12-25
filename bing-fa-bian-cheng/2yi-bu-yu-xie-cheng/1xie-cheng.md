# Python协程：从yield/send到async/await
## 什么是协程
根据维基百科给出的定义，“协程 是为非抢占式多任务产生子程序的计算机程序组件，协程允许不同入口点在不同位置暂停或开始执行程序”。

Python中的协程经历了很长的一段发展历程。其大概经历了如下三个阶段：
```
1.最初的生成器变形yield/send      
2.在python3.3里面引入了yield from关键字
2.在python3.4里面引入了asyncio.coroutine
3.在Python3.5版本中引入async/await关键字
 
```
