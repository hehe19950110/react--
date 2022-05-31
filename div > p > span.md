### 所有的组件 建议首字母大写

#### 在JS里，设置层级递进关系： div > p > span。
```
const div = document.createElement('div')
const p = document.createElement('p')
const span = document.createElement('span')
div.appendChild(p)
p.appendChild(span)
span.innerText = '我是span'

document.body.appendChild(div);
```

> #### 优化1 设置function函数，使上面的声明函数简化。
```
const div = createElement('div')
const p = createElement('p')
const span = createElement('span')
div.appendChild(p)
p.appendChild(span)
span.innerText = '我是span'

document.body.appendChild(div);

function createElement(tagName) {
  return document.createElement(tagName)
}
```

> #### 优化2 在元素内部创建子元素
```
const div = createElement('div', 
              createElement('p',
                createElement('span')))

document.body.appendChild(div);

function createElement(tagName, children) {
  const element = document.createElement(tagName)
  if (children) {
    element.appendChild(children)
  }
  return Element
}
```

> #### 优化3 在元素内部的子元素中，创建文本格式及文本内容
```
const div = createElement('div', 
              createElement('p',
                createElement('span', '我是span里的文本内容')))

document.body.appendChild(div);

function createElement(tagName, children) {
  const element = document.createElement(tagName)
  if (children) {
    if (typeof children === 'string') {
      var childElement = document.createTextNode(children)
      element.appendChild(childElement)
    } else {
      element.appendChild(children)      
    }
  }
  return element
}
```

> #### 优化4 简化部分过长的元素，将其用等简单的单词或字母指代；t代替createElement
```
const div = t('div', 
              t('p',
                t('span', '我是span里的文本内容')))

document.body.appendChild(div);

function t(tagName, children) {
  const element = document.createElement(tagName)
  if (children) {
    if (typeof children === 'string') {
      var childElement = document.createTextNode(children)
      element.appendChild(childElement)
    } else {
      element.appendChild(children)      
    }
  }
  return element
}
```

> #### 优化5 react写法，写一个虚拟的dom 
```
import React from 'react';
import ReactDOM from ‘react-dom';
/***
./react 表示当前加载当前目录下的react文件，；
‘react’ 表示这是一个模块，这是webpack处理的模块，webpack会从node_modules里面去加载这个npm包
***/

const div = (
  React.createElement('div', null,
    React.createElement('p', null,
      React.createElement('span', null, 'hi')))
)
/***
等同于
 const div = ( 
   <div> 
     <p>
       <span>hi</span>
     </p>
   </div>
 )
***/
console.log(div) //element，是虚拟的element

ReactDOM.render(div, document.getElementById('root'))
```

> #### 优化6 react写法，用JS的写法 写组合
```
import React from 'react';
import ReactDOM from ‘react-dom’;

// const div = (
//   React.createElement('div', null,
//     React.createElement('p', null,
//       React.createElement('span', null, 'hi')))
// )
const Header = (
  <header>header</header>
)
const Header2 = function(props) {
  return (
    <header>header {props.name} </header>
  )
}

const Bottom = (
  <div>bottom</div>
)
const div = ( 
  <div> 
    {Header}
    {Header2( {name:'jack'})}
    <Header2 name = "jack" /> 
    <p>
      <span>hi</span>
    </p>
    {Bottom}
  </div>
)
console.log(div) //element，是虚拟的element

ReactDOM.render(div, document.getElementById('root'))
```
