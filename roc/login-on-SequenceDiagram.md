# UML建模 时序图-Sequence Diagram
陈鹏-2014年11月12日 13:48:43
***
##简介
`时序图`，也成为`序列图`或`循序图`，是一种UML行为图。它通过描述对象之间发送`消息`的**时间**顺序显示多个对象之间的动态协作。它可以表示用例的行为顺序，当执行一个用例行为时，时序图中的每条消息对应了一个类操作或状态机中引起转换的触发事件。

##时序图元素
时序图包括如下元素：**对象（Actor），生命线（Life line），控制焦点（Focus of control）和消息（Message）**。

**消息**
消息一般分为同步消息（Synchronous Message），异步消息（Asynchronous Message）和返回消息（Return Message）

##我做的一个例子

![登陆的时序图，带有缓存MemCached][1]


  [1]: http://cshijiel.github.io/roc/images/login-Sequence.png