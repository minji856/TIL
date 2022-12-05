# 리액트
# 함수로 class로 컴포넌트 설정.

```
import React from "react";

class EventComponent1 extends React.Component {
    state = {value : 0, name : this.props.name}
    clickHandler = () => {   
        this.setState({value : this.state.value + 1, name : '이벤트 처리 중'});     
    }
    render() {
        return (
            <div>
                <div>{this.state.value} - {this.state.name}</div>
                <button onClick={this.clickHandler}>Click Button</button>
                <div>전달받은 name = {this.state.name}</div>        {/*전달받은 props를 바꿔보자. 
                맨 처음에 받아와서 state = {name : this.props.name} 여기서 조회하고, 후에 바꾼다 (??)*/}
            </div>
        ) // return
    } // render
} // class

export default EventComponent1;
```

```
import { useState } from "react";
function EventComponent2(p) {
    const[value, setValue] = useState(0);
    const[name, setName] = useState(p.name);  
    const clickHandler = () => {
        setValue(value + 1);
        setName("이벤트 처리 중");
    }
    return (
        <div>
            <div>{value} - {name}</div>
            <button onClick={clickHandler}>Click Button</button>
            <div>전달받은 name = {name}</div>
        </div>
    ) // return

}

export default EventComponent2;
```















