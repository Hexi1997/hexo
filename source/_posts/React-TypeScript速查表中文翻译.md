---
title: React-TypeScript速查表中文翻译
date: 2021-05-14 15:40:47
tags: [大前端,react,ts]
categories: react
---
译者注：react和ts结合使用的系统总结网上相关资料不多，于是就硬着头皮啃了英文版。译者英语烂的要死，勉强翻得。有不对的地方请多多指正。翻译自：https://react-typescript-cheatsheet.netlify.app/docs/basic/setup/
英语基础好的推荐优先去看英文文档
翻译时间截至到2021年4月20日
# 一、基础

## （一）开始之前

### 1、要求

* React基础扎实
* 了解TypeScript

### 2、Import React

```javascript
import * as React from "react";
import * as ReactDOM from "react-dom";
```

这里最推荐的导入react和react-dom的方式，如果在你的tsconfig.json中`allowSyntheticDefaultImports`为true，那么也可以使用`import React from 'react'`来导入。值得一提的是，在create-react-app脚手架的tsconfig.json中，默认将`allowSyntheticDefaultImports`设置为true

## （二） 开始

### 1、组件属性定义（Props)

#### （1）Demo

```js
//使用/**/注释的话编写代码时可以看到提示
type AppProps = {
  message: string;
  count: number;
  disabled: boolean;
  /** 字符串数组 */
  names: string[];
  /** ts联合类型，status取值只能为'waiting' 或者 'success' */
  status: "waiting" | "success";
  /** 任何object类型，不常见，但是经常用于placeholder占位符 */
  obj: object;
  /** 基本等同于object,与'Object'完全等价 */
  obj2: {};
  /** 对象类型，对象内有两个必选属性id和title,类型都是string */
  obj3: {
    id: string;
    title: string;
  };
  /** 对象类型数组集合，对象有两个必选属性id和title，类型都是string */
  objArr: {
    id: string;
    title: string;
  }[];
  /** 方法，返回值为void，没有参数 */
  onClick: () => void;
  /** 方法，返回值为void，有一个参数id,类型为number */
  onChange: (id: number) => void;
  /** react合成事件HTMLButtonElement是点击对象，如果点击div，则是HTMLDivElement */
  onClick(event: React.MouseEvent<HTMLButtonElement>): void;
  /** ？代表该属性可选 */
  optional?: OptionalType;
  /** 不推荐，当children是数组时无法接收*/
  children1: JSX.Element;
  /** 不推荐，当children是字符串时无法接收*/
  children2: JSX.Element | JSX.Element[];
  /** 推荐，接收任何children，空对象除外*/
  children: React.ReactNode;
  /** 推荐，其实就是方法从父组件通过props传递到子组件，*/
  functionChildren: (name: string) => React.ReactNode;
  /** 表单内输入项的change事件*/
  onChange?: React.FormEventHandler<HTMLInputElement>;
  /** props属性全部转发,ref除外*/
  props: Props & React.ComponentPropsWithoutRef<"button">;
  /** props属性全部转发,包括ref*/
  props: Props & React.ComponentPropsWithRef<MyButtonWithForwardRef>;
};
```

#### （2）注意React.ReactNode的一个问题

```js
type Props = {
  children: React.ReactNode;
};

function Comp({ children }: Props) {
  return <div>{children}</div>;
}
function App() {
  //App的children为React.ReactNode类型，输入不能是空对象{{}}，会报错
  return <Comp>{{}}</Comp>; // Runtime Error: Objects not valid as React Child!
}
```

#### （3）选择JSX.Element还是React.ReactNode还是React.ReactElement?

详见https://github.com/typescript-cheatsheets/react/issues/129。

ReactElement是具有类型和属性的对象。

```
 interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>> {
    type: T;
    props: P;
    key: Key | null;
}
```

ReactNode是ReactElement，ReactFragment，字符串，ReactNodes的数字或数组，或者为null，未定义或布尔值：

```js
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;

interface ReactNodeArray extends Array<ReactNode> {}
type ReactFragment = {} | ReactNodeArray;

type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;
```

JSX.Element继承自ReactElement

```js
declare global {
  namespace JSX {
    interface Element extends React.ReactElement<any, any> { }
  }
}
```

例如：

```js
 <p> // <- ReactElement = JSX.Element
   <Custom> // <- ReactElement = JSX.Element
     {true && "test"} // <- ReactNode
  </Custom>
 </p>
```

> 为什么类组件的render方法返回ReactNode，而函数组件返回ReactElement？

确实，他们确实返回了不同的东西。`Component`返回：

```js
 render(): ReactNode;
```

函数是“无状态组件”：

```js
 interface StatelessComponent<P = {}> {
    (props: P & { children?: ReactNode }, context?: any): ReactElement | null;
    // ... doesn't matter
}
```

#### （4）interface(接口）和type（类型别名）该如何选择

* interface和type定义时属性分隔用逗号或者分号都可以

  ```js
  interface IProps {
    name:string,
    age:number
  }
  
  type UserType = {
    name:string;
    age:number;
  }
  ```

* 编写库或者第三方环境类型定义时，优先使用interface，因为他支持合并声明

* 组件的Props和State优先使用type，因为他更受约束

* 详见https://blog.csdn.net/qq_41499782/article/details/112569228

* 总结

  * 语法不一样type UserType = {}  interface IUserType {}

  * 拓展方式不同

    * 接口可以使用 `extends` 关键字来进行扩展（这个继承是包含关系，如果父级有了，子集不可以声明重复的，会报错的），或者是 `implements`来进行实现某个接口
    * 类型别名也可以进行扩展，使用 `&`符号进行（这个继承是合并关系，如果父级有了一个类型，子集还可以声明，但是类型就是变成 &；）这个叫做`交叉类型`

  * 合并声明方式不同

    * 接口可以定义一个名字，后面的接口也可以直接使用这个名字，自动合并所有的声明，可以理解类似为继承，但是不建议这么使用，还是使用`extends`关键字好
    * 类型别名不能重复定义，会直接报错

  * 实例类型进行赋值

    * 接口 没有这个功能

    * 类型别名 可以使用 typeof 获取实例的 类型进行赋值(下面的代码在浏览器环境下)

    * ```js
      let div = document.createElement('div');
      type B = typeof div
      ```

  * 类型映射

    * 接口 没有这个功能

    * 类型别名 可以通过 `in`来实现类型映射，如下：

    * ```js
      type Keys = "firstname" | "lastname"
      type DudeType = {
        [key in Keys]: string
      }
      const test: DudeType = {
        firstname: "323",
        lastname: "332"
      }
      ```

  * 最大的不同

    * 接口可以被类实现，而类型别名不可以

  * 接口可以继承自类，表示该类的所有成员都在接口中。类型别名不行

### 2、函数组件(Function Component)

#### （1）Demo

```js
// 类型别名
type AppProps = {
  message: string;
};

//最简单的声明方式，返回值应该是被ts自动类型推论
const App = ({ message }: AppProps) => <div>{message}</div>;

// 返回值是JSX.Element类型
const App = ({ message }: AppProps): JSX.Element => <div>{message}</div>;

// 一行写
const App = ({ message }: { message: string }) => <div>{message}</div>;
```

以前函数组件还有React.FC写法，现在仍然可以使用（但是不推荐）

### 3、类组件(Class Component)

#### （1）Demo

```js
type MyProps = {
  message: string;
};
type MyState = {
  count: number;
};
class App extends React.Component<MyProps, MyState> {
  state: MyState = {
    count: 0,
  };
  render() {
    return (
      <div>
        {this.props.message} {this.state.count}
      </div>
    );
  }
}
```

#### （2）Props和State的接口或者类型别名不再需要readonly标识其只读

```js
type MyProps = {
  readonly message: string;
};
type MyState = {
  readonly count: number;
};
```

上面这种写法已经过时了，源代码中已经设置了State和Props只读，不需要readonly设置

#### （3）类组件中方法定义

与js写法基本一致，只是方法参数必须标注类型

```js
class App extends React.Component<{ message: string }, { count: number }> {
  state = { count: 0 };
  render() {
    return (
      <div onClick={() => this.increment(1)}>
        {this.props.message} {this.state.count}
      </div>
    );
  }
  //不需要设置返回值类型，只要给参数标注类型即可
  increment = (amt: number) => {
    // like this
    this.setState((state) => ({
      count: state.count + amt,
    }));
  };
}
```

#### （4）类组件中属性定义

如果你需要在类中定义一个属性，像state一样声明即可，不必初始化

```js
class App extends React.Component<{
  message: string;
}> {
  //如此即可
  pointer: number;
  componentDidMount() {
    this.pointer = 3;
  }
  render() {
    return (
      <div>
        {this.props.message} and {this.pointer}
      </div>
    );
  }
}
```

### 4、Hooks

#### （1）useState

* 基本数据类型会自动进行类型推论

```js
//val会被自动类型推论为boolean类型，toggle方法只会接收boolean类型实参
const [val, toggle] = React.useState(false);
```

* 很多数据类型给初始化为null，这就需要联合类型

```js
//使用联合类型让user可被初始化为null
const [user, setUser] = React.useState<IUser | null>(null);
//设置IUser接口的实例对象
setUser(newUser);
```

也可以这样初始化

```js
//使用{}
const [user, setUser] = React.useState<IUser>({} as IUser);
setUser(newUser);
```

#### （2）useReducer

```js
const initialState = { count: 0 };
//联合类型限制type取值范围
type ACTIONTYPE =
  | { type: "increment"; payload: number }
  | { type: "decrement"; payload: string };
//typeof 直接获取实例的类型
function reducer(state: typeof initialState, action: ACTIONTYPE) {
  switch (action.type) {
    case "increment":
      return { count: state.count + action.payload };
    case "decrement":
      return { count: state.count - Number(action.payload) };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = React.useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "decrement", payload: "5" })}>
        -
      </button>
      <button onClick={() => dispatch({ type: "increment", payload: 5 })}>
        +
      </button>
    </>
  );
}
```

#### （3）useEffect

与js并无差别

这里主要需要注意的是，useEffect 传入的函数，它的返回值要么是一个**方法**（清理函数），要么就是**undefined**，其他情况都会报错。

比较常见的一个情况是，我们的 useEffect 需要执行一个 async 函数，比如：

```js
// ❌ 
// Type 'Promise<void>' provides no match 
// for the signature '(): void | undefined'
useEffect(async () => {
  const user = await getUser()
  setUser(user)
}, [])
```

虽然没有在 async 函数里显式的返回值，但是 async 函数默认会返回一个 Promise，这会导致 TS 的报错。

推荐这样改写：

```js
useEffect(() => {
  const getUser = async () => {
    const user = await getUser()
    setUser(user)
  }
  getUser()
}, [])
```

#### （4）useRef

有三种方式初始化useRef

```js
//null!中的感叹号标识之后不会对ref.current做检查，可以使用
const ref1 = useRef<HTMLElement>(null!);
//null，渲染时将ref2挂载到组件上此时ref2.current将会有值，在使用ref2.current之前需要判断if(ref2 && ref2.current)才可以使用ref2.current
const ref2 = useRef<HTMLElement>(null);
//联合类型HTMLElement和null， 同ref2相同，使用前也需要判断ref3 && ref3.current
const ref3 = useRef<HTMLElement | null>(null);
```

```js
import React from "react"

function TextInputWithFocusButton() {
  const inputEl = React.useRef<HTMLInputElement|null>(null);
  const onButtonClick = () => {
    inputEl.current && inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

#### （5）useImperativeHandle

略，不常用

#### （6）自定义hook中注意事项

如果想在自定义hook中返回数组，ts会把数组自动类型推论为联合类型。而不是数组内每一个元素拥有自己的类型。因此需要使用as const作处理。

React团队在返回多项数据时使用object类型而不是tuples（元组）类型。

```js
export function useLoading() {
  const [isLoading, setState] = React.useState(false);
  const load = (aPromise: Promise<any>) => {
    setState(true);
    return aPromise.finally(() => setState(false));
  };
  //如果不使用as const，那么会被类型推论为联合类型（boolean | typeof load），加上as const可以让数组内每个元素拥有自动的类型isLoading是boolean类型，load是typeof load
  return [isLoading, load] as const;
}
```

* 使用第三方hooks库，也需要第三方库导出了ts类型，例如阿里的ahook就支持tshttps://github.com/alibaba/hooks/blob/master/packages/hooks/src/index.ts

### 5、defaultProps设置

#### （1）ts+react你可能不需要defaultProps

根据这条推特https://twitter.com/dan_abramov/status/1133878326358171650，defaultProps将要在未来被抛弃，

解决办法就是使用在入口处使用对象默认值

```js
//函数组件
type GreetProps = { age?: number };
//设置age默认值为21
const Greet = ({ age = 21 }: GreetProps) => // etc
```

```js
//类组件
type GreetProps = {
  age?: number;
};

class Greet extends React.Component<GreetProps> {
  render() {
    //解构的时候赋予默认值
    //如果this.props.age为undefined会使用默认值21
    const { age = 21 } = this.props;
    /*...*/
  }
}

let el = <Greet age={3} />;
```

#### （2）如果你非要用defaultProps

```js
//函数组件
type GreetProps = { age: number } & typeof defaultProps;
const defaultProps = {
  age: 21,
};

const Greet = (props: GreetProps) => {
  // etc
};
Greet.defaultProps = defaultProps;
```

```js
//类组件中
type GreetProps = typeof Greet.defaultProps & {
  age: number;
};

class Greet extends React.Component<GreetProps> {
  static defaultProps = {
    age: 21,
  };
  /*...*/
}

let el = <Greet age={3} />;
```

### 6、表单和事件对象(Forms and Event)

#### （1）默认会自动进行类型推论

```js
const el = (
  <button
    onClick={(event) => {
      /* event将会进行自动类型推论，无需指定 */
    }}
  />
);
```

```js
type State = {
  text: string;
};
class App extends React.Component<Props, State> {
  state = {
    text: "",
  };

  // 事件响应函数分开写，e的类型是React.FormEvent<HTMLInputElement>
  onChange = (e: React.FormEvent<HTMLInputElement>): void => {
    this.setState({ text: e.currentTarget.value });
  };

  //也可以这样写
  onChange: React.ChangeEventHandler<HTMLInputElement> = (e) => {
    this.setState({text: e.currentTarget.value})
  }
  
  render() {
    return (
      <div>
        <input type="text" value={this.state.text} onChange={this.onChange} />
      </div>
    );
  }
}
```

#### （2）表单onSubmit事件

如果不关系合成事件e的具体类型，可以使用通用类型React.SyntheticEvent

```js
<form
  ref={formRef}
  onSubmit={(e: React.SyntheticEvent) => {
    e.preventDefault();
    const target = e.target as typeof e.target & {
      email: { value: string };
      password: { value: string };
    };
    const email = target.email.value;
    const password = target.password.value;
    // etc...
  }}
>
  <div>
    <label>
      Email:
      <input type="email" name="email" />
    </label>
  </div>
  <div>
    <label>
      Password:
      <input type="password" name="password" />
    </label>
  </div>
  <div>
    <input type="submit" value="Log in" />
  </div>
</form>
```

### 7、上下文(Context)

#### （1）Demo

```js
import * as React from "react";
//定义context接口
interface AppContextInterface {
  name: string;
  author: string;
  url: string;
}
//使用联合类型初始化为null
const AppCtx = React.createContext<AppContextInterface | null>(null);
//赋值
const sampleAppContext: AppContextInterface = {
  name: "Using React Context in a Typescript App",
  author: "thehappybug",
  url: "http://www.example.com",
};

export const App = () => (
  //通过Provider挂载到父节点
  <AppCtx.Provider value={sampleAppContext}>...</AppCtx.Provider>
);


export const PostInfo = () => {
  //子孙节点通过useContext、Class.contextType或者Context.Consumer都可以获取到数据
  //优先使用useContext，方便
  const appContext = React.useContext(AppCtx);
  return (
    <div>
      //也可以用appContext!.name，表示不对appContext做检查
      Name: {appContext?.name}, Author: {appContext?.author}, Url:{" "}
      {appContext?.url}
    </div>
  );
};
```

#### （2）拓展

* 使用空对象作为默认值

```js
interface ContextState {
  name: string | null;
}
const Context = React.createContext({} as ContextState);
```

### 8、ref相关

#### （1）ref

```js
class CssThemeProvider extends React.PureComponent<Props> {
  //HTMLDivElement是ref挂载的对象
  private rootRef = React.createRef<HTMLDivElement>();
  render() {
    return <div ref={this.rootRef}>{this.props.children}</div>;
  }
}
```

#### （2）forwardRef

```js
type Props = { children: React.ReactNode; type: "submit" | "button" };
export type Ref = HTMLButtonElement;
export const FancyButton = React.forwardRef<Ref, Props>((props, ref) => (
  <button ref={ref} className="MyClassName" type={props.type}>
    {props.children}
  </button>
));
```

### 9、Portals

#### （1）类组件Demo

```js
//Portals可以将元素挂载到更高的层级，类似modal，每个组件都可以显示在屏幕中间，但是组件的范围并没有这么大，因此需要给index.html中新增一个节点，用来挂载modal，antd就是这样做的
const modalRoot = document.getElementById("modal-root") as HTMLElement;

export class Modal extends React.Component {
  el: HTMLElement = document.createElement("div");

  componentDidMount() {
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }

  render() {
    return ReactDOM.createPortal(this.props.children, this.el);
  }
}
```

#### （2）函数组件

```js
import React, { useEffect, useRef } from "react";
import { createPortal } from "react-dom";

const modalRoot = document.querySelector("#modal-root") as HTMLElement;

const Modal: React.FC<{}> = ({ children }) => {
  const el = useRef(document.createElement("div"));

  useEffect(() => {
    const current = el.current;
    modalRoot!.appendChild(current);
    return () => void modalRoot!.removeChild(current);
  }, []);

  return createPortal(children, el.current);
};

export default Modal;
```

### 10、错误处理(Error Boundaries)

#### （1）什么是Error Boundaries

* 部分UI组件中的JavaScript错误不应该破坏整个应用程序。为了解决React用户的这个问题，React 16引入了一个新的概念Error Boundaries
* Error Boundaries是整个react组件去捕捉其所有子组件中的js错误，并且将其记录，用一个回退的组件去显示出来，而不是使整个的react组件树崩溃。
* 增加Error boundaries能够增加更好的用户体验，如果你的组件中某个组件发生错误，但是这个发生错误的组件并不会去影响其他的组件中的交互功能。

#### （2）使用react-error-boundary库

#### （3）自定义

```js
import React, { Component, ErrorInfo, ReactNode } from "react";

interface Props {
  children: ReactNode;
}

interface State {
  hasError: boolean;
}

class ErrorBoundary extends Component<Props, State> {
  public state: State = {
    hasError: false
  };

  public static getDerivedStateFromError(_: Error): State {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  public componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    console.error("Uncaught error:", error, errorInfo);
  }

  public render() {
    if (this.state.hasError) {
      return <h1>Sorry.. there was an error</h1>;
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

### 11、Concurrent React/React Suspense(异步加载)

暂无

## （三）故障排除手册(Troubleshooting Handbook)

### 1、类型错误（Types)

* 避免使用any

#### （1）联合类型

```js
class App extends React.Component<
  {},
  {
    //使用联合类型，让他可使其初始化为null
    count: number | null;
  }
> {
  state = {
    count: null,
  };
  render() {
    return <div onClick={() => this.increment(1)}>{this.state.count}</div>;
  }
  increment = (amt: number) => {
    this.setState((state) => ({
      //值有可能为null，因此为null时取值0，也可以在解构时赋予默认值
      count: (state.count || 0) + amt,
    }));
  };
}
```

#### （2）类型守护（Type Guarding)

有时联合类型A|B中A和B都是object时，代表A或者B或者两者兼有。如果你期望时A类型的时候，会引起一些混乱。使用in方法、自定义类型守卫进行判断 https://zhuanlan.zhihu.com/p/108856165

```js
interface Admin {
  role: string;
}
interface User {
  email: string;
}

// 方法1、使用in关键字
function redirect(user: Admin | User) {
  if ("role" in user) {
    // use the `in` operator for typeguards since TS 2.7+
    routeToAdminPage(user.role);
  } else {
    routeToHomePage(user.email);
  }
}
//方法2、老版本ts中不存在in关键字，可以自定义了类型守卫判断，详见https://zhuanlan.zhihu.com/p/108856165
function isAdmin(user: Admin | User): user is Admin {
  return (user as any).role !== undefined;
}
```

#### （3）可选类型

```js
class MyComponent extends React.Component<{
  //message可选
  message?: string;
}> {
  render() {
    const { message = "default" } = this.props;
    return <div>{message}</div>;
  }
}
```

* 尽量避免使用!和any

#### （4）枚举类型Enum

* react+ts中尽量避免使用Enum，他会存在一些问题https://fettblog.eu/tidy-typescript-avoid-enums/
* 可以使用字符串union替代枚举

```js
export declare type Position = "left" | "right" | "top" | "bottom";
```

* 枚举默认返回值时是number

```js
export enum ButtonSizes {
  default = "default",
  small = "small",
  large = "large",
}

// 使用，ButtonSizes.default返回default字符串
export const PrimaryButton = (
  props: Props & React.HTMLProps<HTMLButtonElement>
) => <Button size={ButtonSizes.default} {...props} />;
```

#### （5）类型断言

有时候作为代码编写者你比ts解析器了解代码，确定当前变量类型比ts解析器认为的更狭窄(narrower)。可以使用as关键字进行断言（类似数据类型强制转换）

```js
class MyComponent extends React.Component<{
  message: string;
}> {
  render() {
    const { message } = this.props;
    return (
      <Component2 message={message as SpecialMessageType}>{message}</Component2>
    );
  }
}
```

#### （6）两个接口相同的结构，如何区分

* 使用brand关键字，并且设置其值为unique symbol

```js
type OrderID = string & { readonly brand: unique symbol };
type UserID = string & { readonly brand: unique symbol };
type ID = OrderID | UserID;

//类型断言
function OrderID(id: string) {
  return id as OrderID;
}
function UserID(id: string) {
  return id as UserID;
}

function queryForUser(id: UserID) {
  // ...
}
// Error, Argument of type 'OrderID' is not assignable to parameter of type 'UserID'
queryForUser(OrderID("foobar"));
```

#### （7）typeof获取实例的类型

```js
const [state, setState] = React.useState({
  foo: 1,
  bar: 2,
});
//使用typeof获取state实例的类型
const someMethod = (obj: typeof state) => {
  setState(obj);
};
```

#### （8）Partial使用(合并对象时常用)

```js
const [state, setState] = React.useState({
  foo: 1,
  bar: 2,
});

const partialStateUpdate = (obj: Partial<typeof state>) =>
  setState({ ...state, ...obj });

partialStateUpdate({ foo: 2 });
```

#### （9）使用的类型react没有导出

有些可以手动获得

```js
//没有导出ButtonProps
import { Button } from "library";
type ButtonProps = React.ComponentProps<typeof Button>;
type AlertButtonProps = Omit<ButtonProps, "onClick">;
const AlertButton: React.FC<AlertButtonProps> = (props) => (
  <Button onClick={() => alert("hello")} {...props} />
);
```

使用ReturnType获取函数返回对象的类型

```js
function foo(bar: string) {
  return { baz: 1 };
}

type FooReturn = ReturnType<typeof foo>; // { baz: number }
```



# 二、 高阶组件(HOC)
todo


# 三、进阶
todo

# 四、向ts迁移
todo
