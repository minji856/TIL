import React from "react";
class BoardHeading extends React.Component {
    // 출력할 값 중 제목행 구성한다.
    // 번호 제목 작성자 조회수
    render() {
        return(
            <thead>
                <tr>
                    <th>번호</th>
                    <th>제목</th>
                    <th>작성자</th>
                    <th>조회수</th>
                    <th colSpan="2">추가동작</th>
                </tr>
            </thead>
        );
    }
}

class Board extends React.Component {
    // BoardList 내부게시물 5개 배열 테이블 내부 출력
    // 단, 출력시 viewCount 1 증가
    // state = {viewcount: this.props.viewcount}
    // 배열 map 반복
    state = {seq : ''}

    render() {
        // 삭제버튼 클릭한 게시물을 게시물 리스트에서 삭제 - 화면 리렌더링
        // 삭제 누른 애가 e에 담겨있다. 부모의 처리함수에 전달한다.
        const deleteHandler = (e) => {
            this.props.deleteBoard(e.target.id);
        }
        
        const updateHandler = (e) => {
            const obj = {seq:e.target.id, title:'수정한 제목', writer: '수정한 작성자', viewcount: 0};
            this.props.updateBoard(obj);    // 부모의 이ㅓㅂ데이트보드로 전해줌.
        }

        const board_tr = this.props.body.map((oneBoard, index) => {
                return(
                    <tr key={oneBoard.seq}>
                        <td>{oneBoard.seq}</td>
                        <td>{oneBoard.title}</td>
                        <td>{oneBoard.writer}</td>
                        <td>{++oneBoard.viewcount}</td>
                        <td><button id={index} onClick={deleteHandler}>삭제</button></td>
                        <td><button id={oneBoard.seq} onClick={updateHandler}>수정</button></td>
                    </tr>
                );
            }); // loop end
            
            return(
                <tbody>{board_tr} </tbody>
                ); // return end
            }
        }

class BoardInsertForm extends React.Component {
    // 제목값, 작성자값 전달받아서 클릭시
    // 번호 : 게시물리스트 갯수 + 1 // 조회수 : 0
    // props.addBoard(게시물1개 객체 전달)
    // state = {seq:"", writer:"", title:"", viewcount:0}
    constructor(props) {
        super(props);
        this.state = {seq:"", writer:"", title:"", viewcount:0}
    }

    render() {
        const changeHandler = (e) => {       // this.setState({title: ev.target.value});
            this.setState({ [e.target.id] : e.target.value })
        }
    
        const insertHandler = () => {
            this.props.addBoard({seq:this.props.boardList.length + 1, writer:this.state.writer, title:this.state.title, viewcount:this.state.viewcount})
        }
        return(
            <div>
                제목: <input type="text" id="title" name="title" onChange={changeHandler}/><br />
                작성자: <input type="text" id="writer" name="writer" onChange={changeHandler}/><br />
                <button onClick={insertHandler}>글 추가</button>
            </div>
        ) 

    }
}
        
class BoardList extends React.Component {
    constructor(props) {
        super(props);
        this.state = {boardList : [
                {seq:1, title:'제목1', writer : "작성자1", viewcount:1 },
                {seq:2, title:'제목2', writer : "작성자2", viewcount:10 },
                {seq:3, title:'제목3', writer : "작성자3", viewcount:0 },
                {seq:4, title:'제목4', writer : "작성자4", viewcount:7 },
                {seq:5, title:'제목5', writer : "작성자5", viewcount:143 }
            ]}
        }
    
    render() {
        // addBoard 구현
        // 1. 입력된 게시물 1개저낟ㄹ
        // 2. 1번 전달 게시물을ㄹ 게시물리스트 추가
        // 3. state 게시물리스트값 변셩

        const addBoard = (newBoard) => {
            this.state.boardList.push(newBoard);        // newBoard 객체를 boardList 배열에 추가
            this.setState({boardList : this.state.boardList});
        }

        // Board : const deleteHandler = (e) => { this.props.deleteBoard(e.target.id); }
        const deleteBoard = (index) => {
            // 함수 사용한다. push, map, filter, splice
            this.state.boardList.splice(index, 1);  // index로부터 1개 지울래용\
            this.setState({boardList : this.state.boardList})
        }

        const updateBoard = (updateBoard) => {
            this.setState({boardList :
                this.state.boardList.map(function(oneBoard) {
                    return updateBoard.seq == oneBoard.seq ? updateBoard : oneBoard; })});

            // 배열에서 반복하면서 수정하겠다고 버튼 클릭한 seq와 반복중인 5개 게시물의 seq값을 비교함.
            // seq가 일치하는 게시물을 찾는다면, 찾은 게시물을 수정 입력 게시물 내용(제목, 작성)으로 변경한다.
            // state에게 알려준다.
        }

        return(
            <div>
                <table border="3">
                    <BoardHeading />
                    <Board body = {this.state.boardList} deleteBoard = {deleteBoard} updateBoard = {updateBoard} />
                </table>
                <br /><hr /><br />
                <BoardInsertForm addBoard={addBoard} boardList={this.state.boardList} />  
            </div>
        )
    }
}

export default BoardList;
