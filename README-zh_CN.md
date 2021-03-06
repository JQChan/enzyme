Enzyme
=======

[![Join the chat at https://gitter.im/airbnb/enzyme](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/airbnb/enzyme?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![npm Version](https://img.shields.io/npm/v/enzyme.svg)](https://www.npmjs.com/package/enzyme) [![License](https://img.shields.io/npm/l/enzyme.svg)](https://www.npmjs.com/package/enzyme) [![Build Status](https://travis-ci.org/airbnb/enzyme.svg)](https://travis-ci.org/airbnb/enzyme) [![Coverage Status](https://coveralls.io/repos/airbnb/enzyme/badge.svg?branch=master&service=github)](https://coveralls.io/github/airbnb/enzyme?branch=master)


Enzyme是一个React的JavaScript测试工具，它可以很容易地断言，操作和遍历React Components的输出.

通过模仿jQuery用于DOM操作和遍历的API，Enzyme的API非常的直观和灵活.

从Enzyme 2.x 或 React < 16 升级
===========

Are you here to check whether or not Enzyme is compatible with React 16? Are you currently using
Enzyme 2.x? Great! Check out our [migration guide](/docs/guides/migration-from-2-to-3.md) for help
moving on to Enzyme v3 where React 16 is supported.

### [安装](/docs/installation/README.md)

To get started with enzyme, you can simply install it via npm. You will need to install enzyme
along with an Adapter corresponding to the version of react (or other UI Component library) you
are using. For instance, if you are using enzyme with React 16, you can run:

```bash
npm i --save-dev enzyme enzyme-adapter-react-16
```

Each adapter may have additional peer dependencies which you will need to install as well. For instance,
`enzyme-adapter-react-16` has peer dependencies on `react` and `react-dom`.

At the moment, Enzyme has adapters that provide compatibility with `React 16.x`, `React 15.x`,
`React 0.14.x` and `React 0.13.x`.

The following adapters are officially provided by enzyme, and have the following compatibility with
React:

| Enzyme Adapter Package | React semver compatibility |
| --- | --- |
| `enzyme-adapter-react-16` | `^16.4.0-0` |
| `enzyme-adapter-react-16.3` | `~16.3.0-0` |
| `enzyme-adapter-react-16.2` | `~16.2` |
| `enzyme-adapter-react-16.1` | `~16.0.0-0 \|\| ~16.1` |
| `enzyme-adapter-react-15` | `^15.5.0` |
| `enzyme-adapter-react-15.4` | `15.0.0-0 - 15.4.x` |
| `enzyme-adapter-react-14` | `^0.14.0` |
| `enzyme-adapter-react-13` | `^0.13.0` |

Finally, you need to configure enzyme to use the adapter you want it to use. To do this, you can use
the top level `configure(...)` API.

```js
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

Enzyme.configure({ adapter: new Adapter() });
```

第三方 Adapters
=============

It is possible for the community to create additional (non-official) adapters that will make enzyme
work with other libraries. If you have made one and it's not included in the list below, feel free
to make a PR to this README and add a link to it! The known 3rd party adapters are:

| Adapter Package | For Library | Status |
| --- | --- | --- |
| [`preact-enzyme-adapter`](https://github.com/aweary/preact-enzyme-adapater) | [`preact`](https://github.com/developit/preact) | (work in progress) |
|[`enzyme-adapter-inferno`](https://github.com/bbc/enzyme-adapter-inferno)|[`inferno`](https://github.com/infernojs/inferno)|(work in progress)|

运行 Enzyme 测试
===========

Enzyme is unopinionated regarding which test runner or assertion library you use, and should be
compatible with all major test runners and assertion libraries out there. The documentation and
examples for enzyme use [mocha](https://mochajs.org/) and [chai](http://chaijs.com/), but you
should be able to extrapolate to your framework of choice.

If you are interested in using enzyme with custom assertions and convenience functions for
testing your React components, you can consider using:

* [`chai-enzyme`](https://github.com/producthunt/chai-enzyme) with Mocha/Chai.
* [`jasmine-enzyme`](https://github.com/FormidableLabs/enzyme-matchers/tree/master/packages/jasmine-enzyme) with Jasmine.
* [`jest-enzyme`](https://github.com/FormidableLabs/enzyme-matchers/tree/master/packages/jest-enzyme) with Jest.
* [`should-enzyme`](https://github.com/rkotze/should-enzyme) for should.js.
* [`expect-enzyme`](https://github.com/PsychoLlama/expect-enzyme) for expect.


[结合Mocha使用](/docs/guides/mocha.md)

[结合Karma使用](/docs/guides/karma.md)

[结合Browserify使用](/docs/guides/browserify.md)

[结合SystemJS使用](/docs/guides/systemjs.md)

[结合Webpack使用](/docs/guides/webpack.md)

[结合JSDOM使用](/docs/guides/jsdom.md)

[结合React Native使用](/docs/guides/react-native.md)

[结合Jest使用](/docs/guides/jest.md)

[结合Lab使用](/docs/guides/lab.md)

[结合Tape和AVA使用](/docs/guides/tape-ava.md)

基础用法
===========

## [浅层渲染](/docs/api/shallow.md)

```javascript
import React from 'react';
import { expect } from 'chai';
import { shallow } from 'enzyme';
import sinon from 'sinon';

import MyComponent from './MyComponent';
import Foo from './Foo';

describe('<MyComponent />', () => {
  it('renders three <Foo /> components', () => {
    const wrapper = shallow(<MyComponent />);
    expect(wrapper.find(Foo)).to.have.lengthOf(3);
  });

  it('renders an `.icon-star`', () => {
    const wrapper = shallow(<MyComponent />);
    expect(wrapper.find('.icon-star')).to.have.lengthOf(1);
  });

  it('renders children when passed in', () => {
    const wrapper = shallow((
      <MyComponent>
        <div className="unique" />
      </MyComponent>
    ));
    expect(wrapper.contains(<div className="unique" />)).to.equal(true);
  });

  it('simulates click events', () => {
    const onButtonClick = sinon.spy();
    const wrapper = shallow(<Foo onButtonClick={onButtonClick} />);
    wrapper.find('button').simulate('click');
    expect(onButtonClick).to.have.property('callCount', 1);
  });
});
```

查看完整 [API 文档](/docs/api/shallow.md)



## [完整DOM渲染](/docs/api/mount.md)

```javascript
import React from 'react';
import sinon from 'sinon';
import { expect } from 'chai';
import { mount } from 'enzyme';

import Foo from './Foo';

describe('<Foo />', () => {
  it('allows us to set props', () => {
    const wrapper = mount(<Foo bar="baz" />);
    expect(wrapper.props().bar).to.equal('baz');
    wrapper.setProps({ bar: 'foo' });
    expect(wrapper.props().bar).to.equal('foo');
  });

  it('simulates click events', () => {
    const onButtonClick = sinon.spy();
    const wrapper = mount((
      <Foo onButtonClick={onButtonClick} />
    ));
    wrapper.find('button').simulate('click');
    expect(onButtonClick).to.have.property('callCount', 1);
  });

  it('calls componentDidMount', () => {
    sinon.spy(Foo.prototype, 'componentDidMount');
    const wrapper = mount(<Foo />);
    expect(Foo.prototype.componentDidMount).to.have.property('callCount', 1);
    Foo.prototype.componentDidMount.restore();
  });
});
```

查看完整 [API 文档](/docs/api/mount.md)


## [静态渲染标记](/docs/api/render.md)

```javascript
import React from 'react';
import { expect } from 'chai';
import { render } from 'enzyme';

import Foo from './Foo';

describe('<Foo />', () => {
  it('renders three `.foo-bar`s', () => {
    const wrapper = render(<Foo />);
    expect(wrapper.find('.foo-bar')).to.have.lengthOf(3);
  });

  it('renders the title', () => {
    const wrapper = render(<Foo title="unique" />);
    expect(wrapper.text()).to.contain('unique');
  });
});
```

查看完整 [API 文档](/docs/api/render.md)


### Future

[Enzyme Future](/docs/future.md)


### 贡献

查看 [贡献指南](/CONTRIBUTING.md)

### 此外

使用`enzyme`的组织或项目可以在[这里](INTHEWILD.md)列表出来.

### License

[MIT](/LICENSE.md)
