## 1. State영역 변수 선언  
- 변수 선언시 state 영역에 추가했을 경우에만 나중에 값변경이 가능하다  
```javascript
state = {
        number:0
    }
```

## 2. State영역 변수값 변경  
- setState 메서드를 사용한다
```javascript
<div>
    {this.state.number}
</div>
<button className="btn btn-danger"
onClick={
    ()=>{
        this.setState({
            number:this.state.number+1
        })
    }
}>증가</button>
<button className="btn btn-danger"
onClick={
    ()=>{
        this.setState({
            number:this.state.number-1
        })
    }
}>감소</button>
```

- 배열타입 데이터에 데이터를 추가하려면 concat 메서드를 사용한다
```javascript
state={
        maxNo:3,
        boards:[
            {
                brdno:1,
                brdwriter:'캔디',
                brdtitle:'안녕',
                brddate:new Date()
            },
            {
                brdno:2,
                brdwriter:'안소니',
                brdtitle:'반갑다',
                brddate:new Date()
            },
            {
                brdno:3,
                brdwriter:'캐서린',
                brdtitle:'리액트 공부중',
                brddate:new Date()
            }
        ]
}

handleSave=(data)=>{
        console.dir(data);

        this.setState({
            maxNo:++this.state.maxNo,
            boards:this.state.boards.concat({
                brdno:this.state.maxNo,
                brddate:new Date(),
                brdwriter:data.brdwriter,
                brdtitle:data.brdtitle
            })
        })
    }
```
