# 理解 React Hooks

[查看原文](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)

## 为什么会有 Hooks

我们知道组件和单项数据流帮助我们将一个大型的 UI 拆分成很多小的，独立的，重用的小块。**然而我们经常无法将一个复杂的组件拆分的更细因为逻辑是有状态的无法被提取到一个方法或是一个组件之中**。这就是开发者们经常说的 React 无法让他们做到”关注点分离“。

这些情况非常常见其中包括动画，表单处理，和外部数据源通讯以及很多其他的我们想要在组件上实现的功能。当我们尝试仅用组件去解决这些问题时，我们会以这些情况而告终：

- 难以重构以及测试的大型组件。
- 不同组件和生命周期方法里的重复逻辑。
- 像 `render props` 和高阶组件这样复杂的模式。

我们认为 Hooks 是解决这些问题最好的方法。Hooks 可以让我们将组件中的逻辑重新组织成可重用且独立的单位。

Hooks 应用了 React 组件中的哲学（明确的数据流以及成分），而不是在多个组件之间。这就是为什么我觉得 Hooks 是原生符合 React 组件模型的。

不像 `render props` 和高阶组件这样的模式，Hooks 并不引入不必要的组件树嵌套，也不需要遭受 `Mixin` 的缺点。

即使你有本能的第一反应，我都鼓励你去公平的尝试和使用它，我觉得你会喜欢它的。

## Hooks 会使你的 React 更臃肿吗？

在我们详细了解 Hooks 之前，你可能会想我们是不是又给 React 增加了更多概念。这是一个公平的批评。我认为，虽然学习它们肯定会有短期的认知成本，但最终的结果将是相反的。

如果 React 社区拥抱了这个提案。那今后当你在编写 React 应用时它会减少很多的你需要考虑的概念。Hooks 可以使你全程使用纯函数组件而不需要 Class，render props 以及高阶组件。

在实现大小方面，Hooks 仅仅增加了 ~1.5kb（min + gzip）的文件。然而这还没有结束，因为当你使用 Hooks 时你的代码可以被更容易的被压缩，同等的代码量在使用 Hooks 实现时要比使用 Class 来的少。

**Hooks 对你的代码不会有破坏性的更改。**即使在新编写的组件中采用了 Hooks，现有的代码也可以继续运行。事实上，这正是我们的建议 —— 不要做任何大的重写！在关键代码中使用 Hooks 是一个好主意。尽管如此，如果您能够使用 16.7 alpha 进行试验，为我们提供关于 Hooks 提议的反馈并报告任何 bug，我们将不胜感激。

## 到底什么才是 Hooks ？

要理解 Hooks，我们需要后退一步，考虑代码重用。

现在我们有很多的方法在我们的 React app 中复用逻辑。我们可以写一些简单的方法用来计算它们。我们同样也可也使用组件（纯函数组件或是 Class 组件）。组件会更加的强大，但是它们需要渲染 UI。这使的它们在分享没有视图的逻辑时十分不方便。这就是为什么我们要停止使用复杂的模式例如 render props 或是高阶组件。是不是有更简单的东西让我们只使用一种方法而不是很多种来解决这个问题？

方法看上去是一个完美解决代码重用的方案。在函数之间移动逻辑所需的工作量最少。