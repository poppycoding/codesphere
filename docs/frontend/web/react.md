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