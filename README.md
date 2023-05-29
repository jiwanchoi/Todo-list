# Chrome todo-list Clone

> 크롬 앱 todo-list 클론 프로젝트  
> https://jiwanchoi.github.io/Todo-list/

</br>

## 1. 제작 기간

- 2023/05/20 ~ 2022/05/30

</br>

## 2. 사용 기술

- HTML5
- CSS
- JavaScript

</br>

## 3. 프로젝트 목표

- To Do 앱 클론 코딩을 통한 JavaScript 기초 학습
- 이전에 배웠던 HTML, CSS, SASS 복습
- 외부 API 활용

</br>

## 4. 주요 기능

- 기본 구현
  - [x] 랜덤 배경 이미지
  - [x] 실시간 시계
  - [x] 로컬 스토리지를 사용한 로그인
  - [x] 로컬 스토리지를 사용한 투두리스트
  - [x] 날씨와 위치 (geolocation)
- 추가 구현
  - [x] 반응형
  - [x] 사용자 리셋
  - [x] 할 일 목록 취소선
  - [x] 격언, 날씨 상세 링크
  - [x] 날씨 상태에 맞는 아이콘 로딩

</br>

## 5. 주요 코드

```javascript
const todoForm = document.querySelector("#todo-form");
const todoInput = todoForm.querySelector("input");
const todoList = document.querySelector("#todo-list");

const TODOS_KEY = "todos";
const savedTodos = localStorage.getItem(TODOS_KEY);

let toDos = [];

function handleTodoSubmit(event) {
  //해당 이벤트에 대한 실행 방지
  event.preventDefault();
  const newTodo = todoInput.value;
  todoInput.value = "";

  const newTodoObj = {
    text: newTodo,
    id: Date.now(),
  };
  toDos.push(newTodoObj);
  paintTodo(newTodoObj);
  saveTodos();
}

//string형식으로 변환하여 로컬저장소에 todo리스트 저장
function saveTodos() {
  localStorage.setItem(TODOS_KEY, JSON.stringify(toDos));
}

function deleteTodo(event) {
  const li = event.target.parentElement;
  li.remove();
  toDos = toDos.filter((toDo) => toDo.id !== parseInt(li.id));
  saveTodos();
}

function paintTodo(newTodo) {
  const li = document.createElement("li");
  li.id = newTodo.id;
  const span = document.createElement("span");
  span.innerText = newTodo.text;

  span.addEventListener("click", checkingToDo);
  const button = document.createElement("button");
  button.innerText = "❌";
  button.addEventListener("click", deleteTodo);
  li.appendChild(span);
  li.appendChild(button);
  todoList.appendChild(li);
}

todoForm.addEventListener("submit", handleTodoSubmit);

function checkingToDo(event) {
  const span = event.target;
  //toggle사용 _ 하나의 설정값으로부터 다른 값으로 전환
  span.classList.toggle("strikethrough");
}

if (savedTodos !== null) {
  const parsedTodos = JSON.parse(savedTodos);
  toDos = parsedTodos;
  parsedTodos.forEach(paintTodo);
}
```
