<template>
    <div class="container">
        <h2>Todo List</h2>
        <div class="input-group" styles="margin-bottom: 10px;">
            <input type="text" class="form-control" placeholder="할일을 입력하세요"
            v-model="name" v-on:keyup.enter="createTodo(name)" >
            <span class="input-group-btn">
                <button class="btn btn-default" type="button" @click="createTodo(name)">추가</button>
            </span>
        </div>
        <ul class="list-group">
            <li class="list-group-item" v-for="(todo, index) in todos" :key="index">
                {{todo.name}}
                <div class="btn-group pull-right" style="font-size: 12px; line-height: 1;">
                    <button type="button" class="btn-link dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                        더보기<span class="caret"></span>
                    </button>
                    <ul class="dropdown-menu">
                        <li><a href="#" @click="deleteTodo(todo)">삭제</a></li>
                    </ul>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default{
        name: 'TodoPage',
        data() {
            return {
                name:null,
                // todos: [{name:'청소'},{name:'블로그 쓰기'},{name: '밥먹기'},{name: '안녕'}]
                todos:[],
            }
        },
        methods: {
            deleteTodo(todo){
                var vm = this
                this.todos.forEach(function(_todo,i,obj){
                    if(_todo.id === todo.id) {
                        vm.$http.delete('ttp://todos.garam.xyz/api/todos/'+todo.id)
                        .then((result) => {
                            obj.splice(i, 1)
                        })
                    }
                })
            },
            createTodo(name){
                if(name != null) {
                    var vm = this;
                    this.$http.defaults.headers.poat['Content-Type'] = 'application/json';
                    // this.todos.push({name:name});
                    this.$http.post('http://todos.garam.xyz/api/todos', {
                        name:name
                    }).then((result) => {
                        vm.todos.puah(result.data);
                    })
                    this.name = null
                }
            },
            getTodos() {
                var vm=this;
                this.$http.get('http://todos.garam.xyz/api/todos')
                .then((result)=> {
                    // console.log(result)
                    vm.todos = result.data.data;
                })
                {/* api 실행되려면 이벤트 발생시나 Vue의 라이프사이클 중에 선택하여 들어가야한다. */}
            }
        },
        mounted() { {/* mounted는 VueComponent가 페이지에 끼워지고(mounted)나서 호출되는 함수 */}
            this.getTodos();
        }
    }
</script>