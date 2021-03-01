# 1. 调用函数时出错

```jsx
render() {
  // Wrong: handleClick is called instead of passed as a reference!
  return <button onClick={this.handleClick()}>{'Click Me'}</button>
}

render() {
  // Correct: handleClick is passed as a reference!
  return <button onClick={this.handleClick}>{'Click Me'}</button>
}
```

- 需要确保在将函数作为参数传递时未调用该函数。

# 2. 错误边界

- 错误边界是在其子组件树中的任何位置捕获 JavaScript 错误、记录这些错误并显示回退 UI 而不是崩溃的组件树的组件。

- 如果一个类组件定义了一个名为 `componentDidCatch(error, info)` 或 `static getDerivedStateFromError() `新的生命周期方法，则该类组件将成为错误边界：

  ```jsx
  class ErrorBoundary extends React.Component {
    constructor(props) {
      super(props)
      this.state = { hasError: false }
    }
  
    componentDidCatch(error, info) {
      // You can also log the error to an error reporting service
      logErrorToMyService(error, info)
    }
  
    static getDerivedStateFromError(error) {
       // Update state so the next render will show the fallback UI.
       return { hasError: true };
     }
  
    render() {
      if (this.state.hasError) {
        // You can render any custom fallback UI
        return <h1>{'Something went wrong.'}</h1>
      }
      return this.props.children
    }
  }
  
  ```

- 之后，将其作为常规组件使用：

  ```jsx
  <ErrorBoundary>
    <MyWidget />
  </ErrorBoundary>
  ```

# 3. `react-dom` 中 render 方法的目的是什么?

- 此方法用于将 React 元素渲染到所提供容器中的 DOM 结构中，并返回对组件的引用。如果 React 元素之前已被渲染到容器中，它将对其执行更新，并且只在需要时改变 DOM 以反映最新的更改。

```jsx
ReactDOM.render(element, container[, callback])
```

- 如果提供了可选的回调函数，该函数将在组件被渲染或更新后执行。