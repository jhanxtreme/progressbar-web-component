    <template>
    <div id="todo">
        <div class="header-title">
            <h2 class="title1">TODO APP</h2>
            <h2 class="title2">Tasks ({{totalTodos}})</h2>
        </div>
        <div class="action-header default-bg">
            <div class="top">
                <input type="text" maxlength="50" v-on:keyup.enter="addTodo()" v-model="newTodo" placeholder="Add new task" />
            </div>
            <div class="bottom">
                <span v-bind:class="{disabled: totalTodos === 0}" class="btn all" v-on:click="completeAll()">All</span><!--
                --> | <span v-bind:class="{disabled: totalTodos === 0 || totalCompleted === 0}" class="btn clear" :disabled="isDisabled" @click="clearAll()">Clear</span>

                <span class="completed-total">Completed: {{totalCompleted}}</span>
            </div>
        </div>
        <ul v-if="totalTodos > 0">
            <li v-for="(todo, index) in todos" class="default-bg">
                <label>
                    <input type="checkbox" v-bind:id="todo._id" v-show="!todo.editable" v-model="todo.completed" :disabled="todo.editable" />
                    <span class="text101" v-show="!todo.editable" v-bind:class="{completed: todo.completed}">{{todo.name}}</span>
                    <input type="text" maxlength="50" class="edit-input" v-show="todo.editable" :value="todo.name" @keyup.enter="saveTodo($event, index)" />
                    <div class="action-buttons">
                        <span href="#" v-show="todo.editable" v-on:click="cancelTodo(index)">Cancel</span><!--
                        --><span href="#" v-show="!todo.editable" v-on:click="editTodo(index)">Edit</span><!--
                        --> | <span href="#" v-on:click="deleteTodo(index)">Remove</span>
                    </div>
                </label>
            </li>
        </ul>
        <div v-else class="message">No tasks yet.</div>
    </div>
</template>

<script>
let storageKey = 'todos';

let todoStorage = {
    get(){
        return localStorage[storageKey] ? JSON.parse(localStorage[storageKey]) : [];
    },
    save(todos){
        localStorage[storageKey] = JSON.stringify(todos);
    }
};

let component = {
    data(){
        return {
            todos: todoStorage.get(),
            newTodo: null,
            backupTodo: null,
            editMode: false,
            selectAll: false
        }
    },
    methods: {
        addTodo(){
            if(typeof this.newTodo != null && typeof this.newTodo == "string" && this.newTodo.trim().length > 0){
                let tempTodos = this.todos;
                let newTask = {
                    _id: Date.now().toString(36),
                    name: this.newTodo,
                    createdAt: new Date().toISOString(),
                    modifiedAt: null,
                    editable: false,
                    completed: false
                };  
                // console.log(newTask);
                tempTodos.unshift(newTask);
                todoStorage.save(tempTodos);
                this.todos = todoStorage.get();
                this.newTodo = null;
            }else{
                console.log("Invalid string");
            }                      
        },
        deleteTodo(index){
            let tempTodos = this.todos;
            tempTodos.splice(index, 1);
            todoStorage.save(tempTodos);
            this.todos = todoStorage.get();
        },
        saveTodo(event, index){
            if(event.target.value.trim().length > 0){
                this.todos[index].name = event.target.value;
                this.todos[index].editable = false;
                this.todos[index].modifiedAt = new Date().toISOString();
                todoStorage.save(this.todos);
                this.todos = todoStorage.get();
            }
        },
        editTodo(index){
            this.backupTodo = this.todos[index];
            this.todos[index].editable = true;
        },
        cancelTodo(index){
            this.todos[index].name = this.backupTodo.name;
            this.todos[index].editable = false;
            this.backupTodo = null;
        },
        completeAll(){
            this.selectAll = !this.selectAll;
            this.todos.forEach(element => element.completed = this.selectAll);
        },
        clearAll(){
            let tempTodos = this.todos.filter(elem => elem.completed === false);      
            todoStorage.save(tempTodos);
            this.todos = todoStorage.get();
            // this.$refs.completeAll.checked = false;
        }
    },
    computed: {
        totalTodos() {
            return this.todos.length;
        },
        isDisabled(){
            return this.todos.length === 0;
        },
        totalCompleted(){
            return this.todos.filter(e=>e.completed === true).length;
        }
    }
    
};

export default component;

</script>

<style scoped>
#todo{
    background-color: #00BCD4;
    margin: 1em auto;
    text-align: justify;
    height: auto;
    width: 450px;
    /* overflow: auto; */
    padding: .85em;
    border-radius: 1px;
    font-family:Arial, Helvetica, sans-serif;
    font-size: 100%;
    border-radius:4px;
}
ul{
    padding:0;
    margin: 0;
    list-style: none;
    position: relative;
}
li{
    padding: .65em;
    position: inherit;
    border-top: 1px #ccc solid;
    border-radius: 1px;
}
h2{
    margin: 0;
    color: #fff;
    font-size: 150%;
    font-weight: 500 !important;
}
.completed{
    text-decoration: line-through;
}
.default-bg{
    background-color: #efefef;
    color: #222;
}
.action-buttons{
    position: absolute;
    right: .5em;
    top: 50%;
    transform: translateY(-50%);
    color:#555;
}
.action-buttons span{
    color: #222;
    font-size: 80%;
    cursor: pointer;
    text-decoration: none;
}
.action-buttons span:hover{
    text-decoration: underline;
}
.action-header{
    padding: .5em;
    border-radius: 4px;
    margin: .5em 0;
    background-color: #efefef;
}
.action-header input{
    border: 0;
}
.action-header .bottom{
    position: relative;
    color:#888;
}
.action-header .bottom .btn{
    padding: .25em;
    margin: .25em 0;
    cursor: pointer;
    text-decoration: underline;
    display: inline-block;
    font-size: 90%;
    color:#222;
}
.action-header .bottom .disabled{
    color:#888;
}
.action-header .top{
    margin-top:.2em;
}
.action-header .top input{
    width:calc(100% - 2em); 
    padding:1em;
    background-color: #f7f7f7;
    margin-bottom: .5em;
    border-radius: 4px;
}

.edit-input{
    padding: .5em;
    width: 70%;
    background-color: #ffffff;
    color: #333;
    border: 1px #d8d8d8 solid;
}
.text101{
    width: 65%;
    display: inline-block;
    overflow: hidden;
    text-overflow: ellipsis;
    line-height: 14px;
    font-size: 90%;
}
.message{
    color: #fff;
    text-align: center;
    padding: 1em;
    font-size: 90%;
}
.completed-total{
    position: absolute;
    right: .1em;
    display: inline-block;
    top: 50%;
    transform: translateY(-50%);
    font-size: 90%;
    color: #555;
}
.header-title{
    position: relative;
}
.header-title h2.title{
    position: absolute;
    left:0;
    top: 0;
}
.header-title h2.title2{
    position: absolute;
    right: 0;
    top: 0;
}
</style>