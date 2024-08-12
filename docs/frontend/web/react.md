在 React 中，`propTypes` 和 `defaultProps` 是两种用于管理组件属性的工具，帮助开发者确保组件接收到正确类型的属性，并提供属性的默认值。

### `propTypes`

`propTypes` 是 React 提供的一种属性类型检查机制。通过定义 `propTypes`，可以确保组件接收到的属性类型是预期的。如果传入的属性类型不正确，React 会在开发环境中给出警告提示。

#### 常用的 PropTypes 类型

- `PropTypes.string`：字符串
- `PropTypes.number`：数字
- `PropTypes.bool`：布尔值
- `PropTypes.array`：数组
- `PropTypes.object`：对象
- `PropTypes.func`：函数
- `PropTypes.node`：可以渲染的任意内容（数字、字符串、元素或数组）
- `PropTypes.element`：React 元素
- `PropTypes.instanceOf`：特定类的实例
- `PropTypes.oneOf`：可以是指定值之一
- `PropTypes.oneOfType`：可以是多种类型之一
- `PropTypes.arrayOf`：特定类型组成的数组
- `PropTypes.objectOf`：特定类型值组成的对象
- `PropTypes.shape`：具有特定形状的对象

### `defaultProps`

`defaultProps` 用于为组件的属性设置默认值。如果父组件没有传递某个属性，组件会使用 `defaultProps` 中定义的默认值。

### 使用示例

以下是一个简单的示例，展示如何在 React 组件中使用 `propTypes` 和 `defaultProps`。

```jsx
import React from 'react';
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    const { name, age, isStudent } = this.props;
    return (
      <div>
        <p>Name: {name}</p>
        <p>Age: {age}</p>
        <p>Is Student: {isStudent ? 'Yes' : 'No'}</p>
      </div>
    );
  }
}

// 定义 propTypes
MyComponent.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
  isStudent: PropTypes.bool
};

// 定义 defaultProps
MyComponent.defaultProps = {
  age: 18,
  isStudent: false
};

export default MyComponent;
```

在这个示例中：

- `propTypes` 定义了 `name` 属性是必需的且必须是字符串，`age` 属性是数字，`isStudent` 属性是布尔值。
- `defaultProps` 定义了 `age` 的默认值为 18，`isStudent` 的默认值为 `false`。

### 总结

- **`propTypes`**：用于属性类型检查，确保组件接收到的属性类型正确。在开发环境中，如果属性类型不匹配，会发出警告。
- **`defaultProps`**：用于设置组件属性的默认值，如果某个属性没有被传递，组件会使用 `defaultProps` 中定义的值。

通过使用 `propTypes` 和 `defaultProps`，可以提高组件的健壮性和可维护性，确保组件按预期工作并提供默认行为。


在 React 中，函数式组件是通过函数的形式定义的，它们可以接收一个名为 `props` 的参数，用于传递数据和回调函数等信息。`props` 是一个对象，包含了从父组件传递过来的所有属性。

### 基本用法
在函数式组件中，`props` 作为参数传入组件函数，然后可以通过 `props` 对象访问传递的属性。例如：

```javascript
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}
```

在这里，`Greeting` 组件接收 `props` 参数，并通过 `props.name` 获取父组件传递的 `name` 属性值，最终在 JSX 中使用该值。

### 解构赋值
为了让代码更简洁，通常会使用解构赋值的方式直接获取 `props` 中的属性：

```javascript
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

这种方式避免了每次使用 `props.` 前缀，代码更为清晰。

### 默认属性
可以为函数式组件设置默认属性值，以应对父组件没有传递某些 `props` 的情况。可以通过组件的 `defaultProps` 属性来实现：

```javascript
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

Greeting.defaultProps = {
  name: 'Guest',
};
```

如果 `name` 属性没有被传递，则会使用 `defaultProps` 中的默认值 `'Guest'`。

### 不变性
在 React 中，`props` 是不可变的，这意味着组件内部不能修改 `props` 的值。组件的唯一任务是根据 `props` 渲染 UI，任何对 `props` 的修改都应该在父组件中完成，通过重新传递新的 `props` 来更新子组件的显示。

总结来说，`props` 是 React 中组件之间传递数据的主要方式，函数式组件通过接收 `props` 来生成动态的 UI。



在 React 中，`refs` 属性用于访问和操作 DOM 元素或组件实例。通过 `refs`，可以直接与 DOM 进行交互，例如获取输入框的值、聚焦元素或调用组件的某个方法。

### 使用 `refs` 的场景
- 需要直接操作 DOM 元素时，比如控制焦点、文本选择、媒体播放等。
- 访问子组件的实例方法或属性。
- 需要处理第三方库无法通过 React 方式处理的情况。

### `refs` 的创建与使用方式

#### 1. **字符串形式的 `refs`**（已不推荐）
最早的 `refs` 创建方式是通过字符串形式，但由于其灵活性和性能问题，React 团队已不推荐使用这种方式。字符串形式的 `refs` 将 `refs` 的名称作为一个字符串传递给组件内的元素或子组件，然后可以通过 `this.refs` 来访问它们。

示例：

```javascript
class MyComponent extends React.Component {
  handleClick = () => {
    // 通过 this.refs 访问 DOM 元素
    this.refs.myInput.focus();
  }

  render() {
    return (
      <div>
        <input type="text" ref="myInput" />
        <button onClick={this.handleClick}>Focus the input</button>
      </div>
    );
  }
}
```

在这个例子中，`this.refs.myInput` 直接引用了输入框的 DOM 元素。

#### 2. **回调形式的 `refs`**（推荐）
回调形式的 `refs` 是一种更强大和灵活的方式。通过提供一个回调函数给 `ref` 属性，当组件挂载时，回调函数会被立即执行，并且接收该 DOM 元素或组件实例作为参数。你可以将该元素或实例存储到组件实例的某个属性中，方便后续使用。

示例：

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myInputRef = null;
  }

  setMyInputRef = (element) => {
    this.myInputRef = element;
  }

  handleClick = () => {
    if (this.myInputRef) {
      this.myInputRef.focus();
    }
  }

  render() {
    return (
      <div>
        <input type="text" ref={this.setMyInputRef} />
        <button onClick={this.handleClick}>Focus the input</button>
      </div>
    );
  }
}
```

在这个例子中，`ref` 回调函数 `setMyInputRef` 将输入框的 DOM 元素保存到 `this.myInputRef` 中。然后可以在其他方法中访问该元素并执行操作。

#### 3. **`React.createRef`**（推荐）
从 React 16.3 开始，React 提供了 `React.createRef()` 作为创建 `refs` 的推荐方式。`createRef` 创建了一个包含 `current` 属性的对象，该属性会自动更新为对应的 DOM 元素或组件实例。

示例：

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myInputRef = React.createRef();
  }

  handleClick = () => {
    if (this.myInputRef.current) {
      this.myInputRef.current.focus();
    }
  }

  render() {
    return (
      <div>
        <input type="text" ref={this.myInputRef} />
        <button onClick={this.handleClick}>Focus the input</button>
      </div>
    );
  }
}
```

这里的 `myInputRef` 是通过 `React.createRef()` 创建的，可以通过 `this.myInputRef.current` 访问 DOM 元素。

### 总结
- **字符串形式的 `refs`** 已不推荐使用，主要由于其性能和维护性问题。
- **回调形式的 `refs`** 提供了更大的灵活性，可以在组件实例中存储和管理 `refs`。
- **`React.createRef()`** 是现代 React 的推荐方式，简洁、易于使用，并且可以与类组件和函数组件配合使用（结合 `useRef` 钩子）。

通过 `refs`，React 组件可以直接与 DOM 或子组件进行交互，实现复杂的 UI 逻辑。


在 React 中，使用回调函数创建 `refs` 时，每次组件重新渲染都会触发该回调函数。这意味着回调函数可能被调用多次，而不是只在初次渲染时调用一次。这种情况有时会导致性能问题或意外的行为。

### 回调函数创建 `refs` 的调用次数问题

使用回调函数创建 `refs` 的典型方式如下：

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myInputRef = null;
  }

  setMyInputRef = (element) => {
    this.myInputRef = element;
  }

  render() {
    return <input type="text" ref={this.setMyInputRef} />;
  }
}
```

在这个例子中，`setMyInputRef` 是一个回调函数，会在组件每次渲染时调用。当组件首次渲染时，回调函数会被调用，将 DOM 元素赋值给 `this.myInputRef`。但是，如果组件由于状态更新或父组件的重新渲染而重新渲染，那么回调函数将再次被调用。

### 问题的具体表现

1. **调用次数**：回调函数在组件挂载时会被调用一次，用来设置 `ref`，但在组件卸载时，回调函数会再次被调用，这次传入的是 `null`。如果组件频繁渲染，回调函数可能多次设置和清除 `ref`。

2. **性能开销**：每次组件重新渲染时都触发 `ref` 回调函数，可能会引起额外的性能开销，特别是在复杂的 UI 中频繁渲染的组件。

3. **不一致的状态**：由于 `ref` 的清除和重设，可能会在某些情况下导致状态不一致或意外的错误，尤其是在回调函数中执行了复杂逻辑时。

### 使用 `createRef` 避免调用次数问题

React 提供了 `createRef` 方法，从根本上解决了回调函数可能引发的调用次数问题。`createRef` 创建的 `ref` 对象在组件的整个生命周期中都是稳定的，不会因为组件的重新渲染而多次被触发或重新分配。

使用 `createRef` 的典型方式如下：

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myInputRef = React.createRef();
  }

  render() {
    return <input type="text" ref={this.myInputRef} />;
  }
}
```

### 优点

1. **稳定性**：`createRef` 创建的 `ref` 对象是稳定的，在整个组件生命周期内都是唯一且不变的。这避免了回调 `ref` 因组件的重新渲染而反复触发的情况。

2. **性能**：使用 `createRef` 可以减少不必要的函数调用，因为它不会因为组件的重新渲染而多次创建或销毁 `ref`，从而降低性能开销。

3. **简洁**：`createRef` 使用方式简洁明了，更容易理解和维护。

### 何时使用回调 `refs`？

尽管 `createRef` 是首选方式，但回调 `refs` 在某些情况下仍然有用，例如：
- 当需要在 `ref` 设置时执行一些副作用（例如，执行动画、启动第三方库）时。
- 需要动态地分配 `ref` 或者根据条件设置多个 `ref`。

### 总结

- **回调函数创建 `refs`** 的方式在组件每次渲染时都会触发，可能导致多次调用和性能问题。
- **`React.createRef()`** 提供了一种更稳定、性能更优的方式来管理 `refs`，避免了不必要的重复调用，推荐在大多数情况下使用它。
- 回调 `refs` 适用于需要在 `ref` 设置时立即执行额外逻辑的场景，但应谨慎使用以避免性能问题。



React 的事件机制是基于合成事件（Synthetic Event）实现的，这是一个跨浏览器的包装，提供了一个一致的 API，并优化了性能。通过这种机制，React 可以高效地处理事件，并且避免了对原生 DOM `ref` 的依赖，尤其是在处理事件时。

### React 事件机制的工作原理

1. **合成事件**：
   React 使用合成事件来封装浏览器的原生事件，这意味着事件处理程序接收到的事件并不是原生的浏览器事件，而是一个合成事件对象。这个对象模仿了原生事件的接口，并且对所有浏览器都是统一的。

2. **事件委托**：
   React 并不直接将事件监听器附加到每个具体的 DOM 元素上，而是使用事件委托的方式将所有事件绑定到根节点（通常是 `document` 或者 `root` 元素）。当一个事件发生时，React 会根据事件类型将它委托到相应的合成事件处理程序，并逐层冒泡，找到需要处理的组件。

3. **自动解绑**：
   React 会在组件卸载时自动解绑相关的事件处理程序，这减少了内存泄漏的风险，并简化了事件的管理。

### 如何避免使用 `ref`

使用 `ref` 直接操作 DOM 是一种相对较低级的操作，通常用于需要访问或操作 DOM 元素的特殊情况。然而，在大多数情况下，React 的事件机制可以帮助我们避免直接使用 `ref`。以下是一些通过 React 事件机制避免使用 `ref` 的常见场景：

1. **通过状态管理 DOM**：
   在 React 中，组件的状态（`state`）通常用于管理和响应用户输入，而不是直接操作 DOM。例如，在表单中获取输入框的值，使用状态可以代替 `ref`。

   示例：

   ```javascript
   class MyComponent extends React.Component {
     constructor(props) {
       super(props);
       this.state = { inputValue: '' };
     }

     handleChange = (event) => {
       this.setState({ inputValue: event.target.value });
     }

     render() {
       return (
         <input type="text" value={this.state.inputValue} onChange={this.handleChange} />
       );
     }
   }
   ```

   在这个例子中，`input` 的值完全由组件的状态控制，避免了使用 `ref` 直接访问 DOM。

2. **事件回调函数**：
   React 的合成事件可以将事件对象传递给事件处理程序。通过事件对象，开发者可以访问事件的目标元素和相关信息，通常不需要 `ref`。

   示例：

   ```javascript
   class MyComponent extends React.Component {
     handleClick = (event) => {
       console.log('Button clicked:', event.target);
     }

     render() {
       return <button onClick={this.handleClick}>Click me</button>;
     }
   }
   ```

   在这个例子中，`event.target` 提供了对点击按钮的引用，而不需要通过 `ref` 来直接访问 DOM 元素。

3. **使用 `key` 属性重新渲染**：
   在某些情况下，开发者可能需要重置或刷新某个 DOM 元素，而不是直接操作它。这时可以通过改变元素的 `key` 属性来强制 React 重新渲染该元素，避免使用 `ref`。

   示例：

   ```javascript
   class MyComponent extends React.Component {
     state = { refresh: false };

     toggleRefresh = () => {
       this.setState({ refresh: !this.state.refresh });
     }

     render() {
       return (
         <div>
           <input key={this.state.refresh ? 'input1' : 'input2'} type="text" />
           <button onClick={this.toggleRefresh}>Refresh Input</button>
         </div>
       );
     }
   }
   ```

   每次点击按钮时，`key` 属性改变，React 会重新渲染 `input` 元素，而无需直接使用 `ref` 来手动控制。

### 总结

React 的事件机制通过合成事件、事件委托和状态管理，使得我们可以高效地处理用户交互，而无需直接操作 DOM，从而避免了对 `ref` 的频繁使用。通过管理状态和使用事件对象，大部分与用户交互相关的逻辑都可以在 React 的虚拟 DOM 层面处理，从而保持代码的简洁性和可维护性。





受控组件和非受控组件

高阶函数和柯里化
