## 记一次 dialogClass 封装
### 背景
在游戏活动中，经常会遇到弹窗的需求但是弹窗的样式和内容都是不同的，重复开发工作量大，代码冗余，于是想封装一个弹窗类，来解决这个问题。

### 思路
不同点
1. 弹窗的样式不同
2. 弹窗的内容不同
3. 弹窗的功能不同
相同点
1. 弹窗的显示和隐藏
2. 弹窗的动效

- 如果弹窗类型少、样式和功能差异不大，且频繁切换显示 ：建议使用一个实例通过不同参数展示不同弹窗，以减少资源占用和简化管理。
- 如果弹窗类型多、样式和功能差异大，且需要独立管理 ：建议使用多个实例展示不同弹窗，以提高代码的可维护性和扩展性。

用户通过 `new Dialog()` 创建一个弹窗示例，传入弹窗的配置对象，然后调用 `show()` 方法显示弹窗，调用 `hide()` 方法隐藏弹窗。 


### 实例化

实例化时，传入配置对象，包含默认的参数

SrcoreDialog.Show()

