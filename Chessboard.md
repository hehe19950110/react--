### 棋盘格效果——1.react绑定一个事件
```
import React from 'react';
import ReactDOM from 'react-dom';
import './style.css';

const Cell = function () {
  const [text, setText] = React.useState('')
  const onClickButton = function() {
    setText('x')
  }
  return (
    <div className="cell" onClick={onClickButton}>
       {text}
    </div>
  )
}

ReactDOM.render(<div>
  <Cell />
</div>,document.getElementById('root'))
```
### 棋盘格效果——2.声明一个数组中的数组
```
import React, { useState } from 'react';
import ReactDOM from 'react-dom';
import './style.css';

const Cell = function () {
  const [text, setText] = React.useState('')
  const onClickButton = function() {
    setText('x')
  }
  return (
    <div className="cell" onClick={onClickButton}>
       {text}
    </div>
  )
}

const cells = [
  [1,1,1],
  [2,2,2],
  [3,3,3],
]

const Chessboard = function() {
  return(
    <div>
      {cells.map( items => <div> 
        {items.map(item => <div>{item}</div>)} 
      </div>)} 
    </div>
  )
}
/***
1. map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。
2. 变量要用{}包裹
3. 第一层，{cells.map()}得到了一个数组，并为数组里的每一项都设置包裹一个div，包裹的时候，并为div里的每一项再包裹一个div。
***/
ReactDOM.render(
  <div>
    <Chessboard />
  </div>,document.getElementById('root')
)
```


// 棋盘格效果——3.设置九宫格 三横三竖的样式
import React, { useState } from 'react';
import ReactDOM from 'react-dom';
import './style.css';

const cells = [
  [1,1,1],
  [2,2,2],
  [3,3,3],
]
const Cell = function(props) {
  return (
    <div className = "cell">
      {props.text}
    </div>
  )
}

const Chessboard = function() {
  return(
    <div>
      {cells.map( items => <div className="row"> 
        {items.map(item => <div className='col'>
          <Cell text={item} />
        </div>)} 
      </div>)} 
    </div>
  )
}
/***
1. map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。
2. 变量要用{}包裹
3. 设置九宫格 棋盘格子：第一层，{cells.map()}得到了一个数组，并为数组里的每一项都设置包裹一个div，包裹的时候，并为div里的每一项再包裹一个div。每一个格子映射成一个Cell。
4. 设置九宫格 三横三竖的样式，三横 row，每个row里 三个col：
{cells.map( items => <div className="row"> 
  {items.map(item => <div className='col'>
    <Cell text={item} />
  </div>)} 
</div>)} 
在style.css设置：
.row {
  display: flex;
}
***/

ReactDOM.render(
  <div>
    <Chessboard />
  </div>,document.getElementById('root')
)



深究useState的原理  https://juejin.cn/post/6867077120691011591

// 棋盘格效果——4.设置九宫格 内部的X
import React, { useState } from 'react';
import ReactDOM from 'react-dom';
import './style.css';

const Cell = function(props) {
  return (
    <div className = "cell" onClick={props.onClick}>
      {props.text}
    </div>
  )
}

const Chessboard = function() {
  const [cells, setCells] = React.useState([
    [1,1,1],
    [2,2,2],
    [3,3,3],
  ])
/***
useState 
在 React中，我们使用 useState 为函数组件设置内部数据，React.useState()接收一个参数作为变量的初始值，返回一个数组，数组的第一项用于变量的读，数组的第二项用于变量的写，
const [n, setN] = React.useState(0);
n 变量的读；setN 变量的写。
***/

  const onClickCell = (row,col)=> {
    console.log('on click cell')
    console.log('行' + row)
    console.log('列' + col)
    const copy = JSON.parse(JSON.stringify(cells))
    copy[row][col] = 'x'
    setCells(copy)
  }
  return(
    <div>
      {cells.map( (items, row) => <div className="row"> 
        {items.map((item, col) => <div className='col'>
          <Cell text={item} onClick={() => onClickCell(row,col)} />
        </div>)} 
      </div>)} 
    </div>
  )
}
/***
1. map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。
2. 变量要用{}包裹
3. 设置九宫格 棋盘格子：第一层，{cells.map()}得到了一个数组，并为数组里的每一项都设置包裹一个div，包裹的时候，并为div里的每一项再包裹一个div。每一个格子映射成一个Cell。
4. 设置九宫格 三行三列的样式，三行 row，每个row里 三个col：
{cells.map( items => <div className="row"> 
  {items.map(item => <div className='col'>
    <Cell text={item} />
  </div>)} 
</div>)} 
在style.css设置：
.row {
  display: flex;
}
5. 函数与箭头函数:任何f 都可以改写成 const f2 = (...args) => f(...args)
  onClick 可以改写成 () => onClickCell()，可以接受参数 但不会立刻执行.
6. <Cell text={item} onClick={() => onClickCell(row,col)} />
当Cell被点击的时候 会调用一个函数，函数就会去调用onClickCell,同时把row与col传递过去，row与col 是用JS的闭包里来的.
7.   const copy = JSON.parse(JSON.stringify(cells))
深拷贝，先声明一个新的地址，把原有内容全拷贝一份，然后先变成字符串，再把字符串变成一个新的对象；Cell 每点击一下，就会生成新的对象，页面就会重新渲染.
***/
ReactDOM.render(
  <div>
    <Chessboard />
  </div>,document.getElementById('root')
)



// 棋盘格效果——5.设置九宫格 判断成功与失败
import React, { useState } from 'react';
import ReactDOM from 'react-dom';
import './style.css';

const Cell = function(props) {
  return (
    <div className = "cell" onClick={props.onClick}>
      {props.text}
    </div>
  )
}

const Chessboard = function() {
  const [cells, setCells] = React.useState([
    [null,null,null],
    [null,null,null],
    [null,null,null],
  ])
  /***
  useState 
  在 React中，我们使用 useState 为函数组件设置内部数据，React.useState()接收一个参数作为变量的初始值，返回一个数组，数组的第一项用于变量的读，数组的第二项用于变量的写，
  const [n, setN] = React.useState(0);
  n 变量的读；setN 变量的写。
  ***/

  const [n,setN] = useState(0)
  const [finished,setFinished] = useState(false)
  const judge = (cells) => {
    //行：
    for(let i=0;i<3;i++) {
      if (cells [i][0] == cells [i][1] && cells [i][1] == cells [i][2] &&
          cells [i][0] !== null) {
          console.log(cells [i][0] + '赢了')
          setFinished(true)
        }
    }
    //竖：
    for(let i=0;i<3;i++) {
      if (cells [0][i] == cells [1][i] && 
		 cells [1][i] == cells [2][i] &&
          cells [0][i] !== null) {
          console.log(cells [0][i] + '赢了')
          setFinished(true)
        }
    }
    //斜：
    if (cells [0][0] == cells [1][1] && 
        cells [1][1] == cells [2][2] &&
        cells [0][0] !== null) {
        console.log(cells[0][0] + '赢了')
        setFinished(true)
        }
    if (cells [0][2] == cells [1][1] && 
        cells [1][1] == cells [2][0] &&
        cells [0][2] !== null) {
        console.log(cells[0][2] + '赢了')
        setFinished(true)
        }
  }
  const onClickCell = (row,col)=> {
    //n+1：
    setN(n + 1)
    //通过深拷贝来改变cells：
    const copy = JSON.parse(JSON.stringify(cells))
    copy[row][col] = n % 2 == 0 ? 'x' : 'o'
    setCells(copy)
    //判断输赢，深拷贝是异步更新，需要同步cells：
    judge(copy)
  }
  return(
    <div>
      {cells.map( (items, row) => <div className="row"> 
        {items.map((item, col) => <div className='col'>
          <Cell text={item} onClick={() => onClickCell(row,col)} />
        </div>)} 
      </div>)} 
      {finished && <div className="gameOver"> 游戏结束 </div>}
    </div>
  )
}
/***
1. map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。
2. 变量要用{}包裹
3. 设置九宫格 棋盘格子：第一层，{cells.map()}得到了一个数组，并为数组里的每一项都设置包裹一个div，包裹的时候，并为div里的每一项再包裹一个div。每一个格子映射成一个Cell。
4. 设置九宫格 三行三列的样式，三行 row，每个row里 三个col：
{cells.map( items => <div className="row"> 
  {items.map(item => <div className='col'>
    <Cell text={item} />
  </div>)} 
</div>)} 
在style.css设置：
.row {
  display: flex;
}
5. 函数与箭头函数:任何f 都可以改写成 const f2 = (...args) => f(...args)
  onClick 可以改写成 () => onClickCell()，可以接受参数 但不会立刻执行.
6. <Cell text={item} onClick={() => onClickCell(row,col)} />
当Cell被点击的时候 会调用一个函数，函数就会去调用onClickCell,同时把row与col传递过去，row与col 是用JS的闭包里来的.
7.   const copy = JSON.parse(JSON.stringify(cells))
深拷贝，先声明一个新的地址，把原有内容全拷贝一份，然后先变成字符串，再把字符串变成一个新的对象；Cell 每点击一下，就会生成新的对象，页面就会重新渲染.
***/

ReactDOM.render(
  <div>
    <Chessboard />
  </div>,document.getElementById('root')
)
