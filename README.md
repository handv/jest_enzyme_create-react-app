# 什么是单元测试

## 单元测试 [维基百科](https://zh.wikipedia.org/wiki/%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
>在计算机编程中，单元测试（英语：Unit Testing）又称为模块测试, 是针对程序模块（软件设计的最小单位）来进行正确性检验的测试工作。程序单元是应用的最小可测试部件。在过程化编程中，一个单元就是单个程序、函数、过程等；***对于面向对象编程，最小单元就是方法，包括基类（超类）、抽象类、或者派生类（子类）中的方法***。
通常来说，程序员每修改一次程序就会进行最少一次单元测试，在编写程序的过程中前后很可能要进行多次单元测试，以证实程序达到软件规格书要求的工作目标，没有程序错误；虽然单元测试不是什么必须的，但也不坏，这牵涉到项目管理的政策决定。
每个理想的测试案例独立于其它案例；为测试时隔离模块，经常使用stubs、mock或fake等测试马甲程序。***单元测试通常由软件开发人员编写，用于确保他们所写的代码匹配软件需求和遵循开发目标***。它的实施方式可以是非常手动的（透过纸笔），或者是做成构建自动化的一部分。

一个软件越容易写单元测试，就表明它的模块化结构越好，模块之间的耦合越弱。React的组件化和函数式编程，天生适合进行单元测试。

## 为什么要写单元测试

1. 测试可以确保得到预期的结果，改代码更自信
2. 有单元测试的代码，通常是更好的代码
    - 写单元测试的时候，会更深入的认识代码；
    - 为了更方便的写单元测试，组件拆分更合理。
3. 所有代码变动都是可预期的
4. 更好的提升自我
    - 写单测的开发更靠谱
    - 更好的吹牛逼（我的单元测试覆盖率是100%）

## 单元测试不是万能的

![image](http://ydschool-online.nos.netease.com/1539337871760unitest.jpg)

单元测试只是测试代码功能，不包含复杂的业务逻辑。

# Jest && Enzyme

## [Jest](https://jestjs.io/docs/zh-Hans/getting-started.html)

Jest是Facebook发布的一个开源的、基于Jasmine框架的JavaScript单元测试工具。提供了包括内置的测试环境DOM API支持、断言库、Mock库等，还包含了Spapshot Testing、 Instant Feedback等特性。用来测试包括React应用在内的所有JavaScript代码。

- 优点

1. 提供控制台实时反馈测试结果
2. 提供内置的测试环境DOM API支持、断言库、Mock库、代码覆盖率报告
3. Snapshot测试：Jest能够对React组件树进行序列化，生成对应的字符串快照，通过比较字符串提供高性能的UI检测
4. 执行速度快
5. 无需配置

使用 create-react-app 或 react-native init 创建你的 React 或 React Native 项目时，Jest 都已经被配置好并可以使用了。在 __tests__文件夹下放置你的测试用例，或者使用 .spec.js 或 .test.js 后缀给它们命名。不管你选哪一种方式，Jest 都能找到并且运行它们。

6. 丰富的api，完善的文档
7. 在线使用

可以使用[repl.it](https://repl.it/repls/CommonPossibleStack)来在线尝试Jest。想想怎么用add ()函数来相加两个数。我们可以编写一个简单的测试，通过 add-test.js来验证 1 + 2 等于 3。输入"run"立马尝试。

## [Enzyme](https://airbnb.io/enzyme/)

Enzyme是Airbnb开源的React测试类库，提供了一套简洁强大的API，并通过jquery风格的方式进行dom处理，开发体验十分友好。不仅在开源社区有超高人气，同时也获得了React官方的推荐。

- 为啥除了Enzyme又要配合Jest呢？

因为要编写测试用例的话，光有测试类库还不够，还需要测试运行环境、断言库、mock库等等工具辅以支持；Jest把这些统统囊括。

## 测试环境搭建

以create-react-app为例

```
npm install --save enzyme enzyme-adapter-react-16 enzyme-to-json react-test-renderer
```

如果React的版本是15或者16, Enzyme需要一个Adapter与React通信。adapter需要全局配置，方法如下:

```
src/setupTests.js
import { configure } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

configure({ adapter: new Adapter() })
```

## 常用api介绍

### [Enzyme api](https://airbnb.io/enzyme/docs/api/)

- shallow渲染

1. shallow: 返回App的浅渲染

浅渲染指的是，将一个组件渲染成虚拟DOM对象，但是只渲染第一层，不渲染所有子组件，所以处理速度非常快。它不需要DOM环境，因为根本没有加载进DOM。

2. find(selector): 返回指定ShallowWrapper组件

```
component.find('.my-class'); // by class name
component.find('#my-id'); // by id
component.find('td'); // by tag
component.find('div.custom-class'); // by compound selector

component.find('[bar=false]'); // by prop selector

component.find(TableRow); // by react component constructor
component.find('TableRow'); // by react component's displayname
```

3. at(index): 返回指定位置的子组件
4. get(index): 返回指定位置的子组件的DOM节点
5. props(): 返回根组件的所有属性
6. prop(key): 返回根组件的指定属性
7. state([key]): 返回根组件的状态
8. setState(nextState): 设置根组件的状态
9. setProps(nextProps): 设置根组件的属性
10. simulate(event[, ...args]): 模拟事件
11. debug()

```
console.log(wrapper.find('Ueditor').debug())
```

它跟shallow方法非常像，主要的不同是采用了第三方HTML解析库Cheerio，它返回的是一个Cheerio实例对象。

- render渲染

1. render: 将React组件渲染成静态的HTML字符串，然后使用Cheerio这个库分析这段HTML代码的结构，返回一个Cheerio对象。

- mount渲染

1. mount: 完全渲染

将React组件加载为真实DOM节点，用到了jsdom来模拟浏览器环境。用于测试：
    - 需要跟dom api交互的组件
    - 被高阶组件包装的组件

### [Jest api](https://jestjs.io/docs/zh-Hans/api)

- globals api

1. describe(name, fn)：描述块，讲一组功能相关的测试用例组合在一起，test suite
2. it(name, fn, timeout)：别名test，用来放测试用例，test case
3. afterAll(fn, timeout)：所有测试用例跑完以后执行的方法
4. beforeAll(fn, timeout)：所有测试用例执行之前执行的方法
5. afterEach(fn)：在每个测试用例执行完后执行的方法
6. beforeEach(fn)：在每个测试用例执行之前需要执行的方法

- expect api

1. expect.toMatchSnapshot()
2. expect.toBeCalledWith(arg1, arg2, ...)
3. expect.toEqual(value)
4. expect.toBe(value)，相当于===

- mock api

1. jest.fn(): mock function
2. jest.fn().mockResolvedValue(value): 异步mock promise的resolve
3. jest.fn().mockRejectedValue(new Error('Async error')): 异步mock promise的reject

## [Jest命令行](https://jestjs.io/docs/zh-Hans/cli)

- 运行所有测试(默认)

```
jest
```

or

```
npm test 
```

- 运行监视模式

```
npm test --watch // 默认运行基于 hg/git (未提交的文件) 修改的文件的测试
npm test --watchAll
```

- 生成测试覆盖率报告

```
npm test --coverage
```

## 测试脚本编写

```
import React from 'react'
import {shallow} from 'enzyme'
import toJson from 'enzyme-to-json'
import App from './App'

describe('App', () => { // test suite
  it('test default props', () => { // test case
    const wrapper = shallow(<App />)
    expect(toJson(wrapper)).toMatchSnapshot()
  })
})
```

## snapshot 

引入'enzyme-to-json'，使用toJson方法，快照会生成一个组件的UI结构。jest在执行的时候，如果发现toMatchSnapshot方法，会在同级目录下生成一个__snapshots__文件夹,以字符串的形式存放快照文件。

以后每次测试的时候都会和第一次生成的快照进行字符串比较来判断UI是否改变。因为是字符串比较，所以性能很高。

```
// 上文中代码生成的snapshot

exports[`App test default props 1`] = `
<div
  className="App"
>
  <header
    className="App-header"
  >
    <img
      alt="logo"
      className="App-logo"
      src="logo.svg"
    />
    <p>
      Edit 
      <code>
        src/App.js
      </code>
       and save to reload.
    </p>
    <a
      className="App-link"
      href="https://reactjs.org"
      rel="noopener noreferrer"
      target="_blank"
    >
      Learn React
    </a>
  </header>
</div>
`;
```

当两次快照内容不同时，可以手动修复问题解决。如果是你期望的改动，则可以直接使jest --updateSnapshot来更新快照文件。也可以在监视模式下直接按'u'来更新快照。

![image](https://jestjs.io/img/content/interactiveSnapshotUpdate.gif)

- 优点

1. 不用写大量的断言判断ui，自动生成
2. 异常时给出详细的错误信息，方便调试
3. 一键更新，无需重写测试用例
4. [万物皆可snapshot](https://medium.com/@newyork.anthonyng/use-jest-snapshot-on-everything-4c5d4c88ca16)

- snapshot不是万能的

1. snapshot不能覆盖100%
2. snapshot不能替代其他单元测试方法



# 测试实战

## what:测什么

- 测试优先级

1. 独立不依赖，常用组件
2. 辅助性组件，utils
3. 更复杂组件

- 不需要测试

1. 第三方库
2. 常量
3. 内联样式(不会改变组件的行为)
4. 高阶组件(HOC)

## how:怎么测

示例
```
class Choice extends PureComponent {
  renderType = (type, index) => {
    const { answer } = this.props
    const value = String.fromCharCode(index + 65)
    switch (type) {
      case 'checkbox':
        return (
          <Checkbox
            value={value}
            checked={answer && answer.includes(value)}
          >
            选项{value}：
          </Checkbox>
        )
      case 'radio':
        return <Radio value={value} checked={value === answer}>选项{value}：</Radio>
      default:
        return `选项${value}：`
    }
  }
  render() {
    const { choice, index, type, onChange, onClick, parentIndex } = this.props
    return (
      <div className={s.choice} >
        <header>
          { this.renderType(type, index) }
        </header>

        <Row type="flex" justify="start">
          <Col>
            <Button icon="delete" onClick={() => onClick(index, parentIndex)} />
          </Col>
        </Row>
      </div>
    )
  }
}

Choice.propTypes = {
  choice: PropTypes.string.isRequired,
  index: PropTypes.number.isRequired,
  onChange: PropTypes.func.isRequired, 
  onClick: PropTypes.func.isRequired, 
  answer: PropTypes.string, 
  type: PropTypes.string, 
  parentIndex: PropTypes.number, 
}

Choice.defaultProps = {
  type: 'text',
  answer: '',
  parentIndex: null,
}

export default Choice
```

- ui测试

snapshot测试

- prop测试

带prop参数的snapshot测试

```
it('default props', () => {
  const wrapper = shallow(
    <Choice
      choice="A"
      index={0}
      onChange={() => null}
      onClick={() => null}
    />
  )
  expect(toJson(wrapper)).toMatchSnapshot()
})

it('test props type=radio', () => {
  const wrapper = shallow(
    <Choice
      choice="A"
      index={0}
      onChange={() => null}
      onClick={() => null}
      type="radio"
      answer="A"
    />
  )
  expect(toJson(wrapper)).toMatchSnapshot()
})

it('test props type=checkbox', () => {
  const wrapper = shallow(
    <Choice
      choice="A"
      index={0}
      onChange={() => null}
      onClick={() => null}
      type="checkbox"
      answer="AB"
    />
  )
  expect(toJson(wrapper)).toMatchSnapshot()
})
```

- event测试

对于事件，按照如下思路测试：

```
mock event => simulate it => expect event was called
mock event => simulate event with params => expect event was called with passed params
pass necessary props => render component => simulate event => expect a certain behavior on called event
```

比如：

```
it('test function onClick', () => {
  const choice = 'C'
  const index = 2
  const parentIndex = 0
  const onClick = jest.fn()
  const wrapper = shallow(
    <Choice
      choice={choice}
      index={index}
      parentIndex={parentIndex}
      onChange={() => null}
      onClick={onClick}
      answer="A"
    />
  )
  wrapper.find('Button').at(0).simulate('click', index, parentIndex)
  expect(onClick).toBeCalledWith(index, parentIndex)
})
```

如果事件中有event参数，比如:

```
handleDifficulty(e) {
  this.handleChange({ difficulty: e.target.value })
}
// 单测中要用{ target: { value } }
it('test function handleDifficulty', () => {
  const value = 1
  wrapper.find('RadioGroup').at(0).simulate('change', { target: { value } })
  expect(onChange).toBeCalledWith({ difficulty: value })
})
```

**备注：对于组件自定义事件，尽量以onXXX命名，才能在simulate的时候触发**

- state测试

```
it('test function onClick', () => {
  const choice = 'C'
  const index = 2
  const parentIndex = 0
  const onClick = jest.fn()
  const wrapper = shallow(
    <Choice
      choice={choice}
      index={index}
      parentIndex={parentIndex}
      onChange={() => null}
      onClick={onClick}
      answer="A"
    />
  )
  wrapper.find('Button').at(0).simulate('click', index, parentIndex)
  expect(wrapper.state('stateValue')).toEqual(null)
})
```
- expect随机数

```
it('will check the matchers and pass', () => {
  const user = {
    createdAt: new Date(),
    id: Math.floor(Math.random() * 20),
    name: 'LeBron James',
  }

  expect(user).toMatchSnapshot({
    createdAt: expect.any(Date),
    id: expect.any(Number),
  })
})

// Snapshot
exports[`will check the matchers and pass 1`] = `
Object {
  "createdAt": Any<Date>,
  "id": Any<Number>,
  "name": "LeBron James",
}
`
```

or 

```
Date = jest.fn(() => 1482363367071)
```

- HOC connect(container)

antd pro推荐[这种做法](https://pro.ant.design/docs/ui-test-cn#%E6%B5%8B%E8%AF%95-dva-%E5%8C%85%E8%A3%85%E7%BB%84%E4%BB%B6)：

被 dva connect 的 React 组件可以使用下面方式进行测试。

```
import React from 'react'
import { shallow } from 'enzyme'
import Dashboard from './Dashboard'

it('renders Dashboard', () => {
  // 使用包装后的组件
  const wrapper = shallow(
    <Dashboard.WrappedComponent user={{ list: [] }} />
  )
  expect(wrapper.find('Table').props().dataSource).toEqual([])
})
```

关于[Component.WrappedComponent](es/react-router/docs/api/withRouter.md#componentwrappedcomponent)

The wrapped component is exposed as the static property WrappedComponent on the returned component, which can be used for testing the component in isolation, among other things.

```
// MyComponent.js
export default withRouter(MyComponent)

// MyComponent.test.js
import MyComponent from './MyComponent'
render(<MyComponent.WrappedComponent location={{...}} ... />)
```

- HOC withRouter(container)

redux官方推荐[这种做法](https://redux.js.org/recipes/writingtests#connected-components)：

```
import { connect } from 'react-redux'
​
// Use named export for unconnected component (for tests)
export class App extends Component { /* ... */ }
​
// Use default export for the connected component (for app)
export default connect(mapStateToProps)(App)
```

- mock dispatch

```
const dispatch = jest.fn().mockResolvedValue('default')
```

## 代码覆盖率

代码覆盖率是一个测试指标，用来描述测试用例的代码是否都被执行。统计代码覆盖率一般要借助代码覆盖工具，Jest内置代码覆盖工具。

- 四个测量维度

1. 行覆盖率(line coverage)：是否测试用例的每一行都执行了
2. 函数覆盖率(function coverage)：是否测试用例的每一个函数都调用了
3. 分支覆盖率(branch coverage)：是否测试用例的每个if代码块都执行了
4. 语句覆盖率(statement coverage)：是否测试用例的每个语句都执行了

# 总结

1. Jest + Enzyme 写单元测试快速方便
2. Snapshot简单易用，在不追求测试覆盖率的情况下，可以简单写几个snapshot
3. 单元测试对于写更好的代码，很有帮助

# 参考文献

1. https://zh.wikipedia.org/wiki/%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95
2. https://jestjs.io/docs/zh-Hans/getting-started.html
3. https://airbnb.io/enzyme/
4. http://react-china.org/t/jest-enzyme-react/11769
5. https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#initializing-test-environment
6. https://www.youtube.com/watch?v=8Ww2QBVIw0I&feature=youtu.be
7. https://github.com/ned-alyona/posts/tree/master/jest-enzyme-testing
8. https://juejin.im/post/5b6c39bde51d45195c079d62
9. http://echizen.github.io/tech/2017/04-24-component-lifycycle-test
10. https://medium.com/@newyork.anthonyng/use-jest-snapshot-on-everything-4c5d4c88ca16
11. https://hackernoon.com/snapshot-testing-react-components-with-jest-744a1e980366
12. https://blog.bitsrc.io/how-to-test-react-components-using-jest-and-enzyme-fab851a43875
13. https://pro.ant.design/docs/ui-test-cn
14. https://cn.redux.js.org/docs/recipes/WritingTests.html
15. https://hackernoon.com/testing-react-components-with-jest-and-enzyme-41d592c174f
16. https://github.com/r-walsh/react-unit-test-practice