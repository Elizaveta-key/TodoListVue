<!-- //содержит основную часть функций списка задач. -->
<script>
    export default {
        data() {
            return {
                todos: JSON.parse(localStorage.getItem("todos") || '[]'),
                id: JSON.parse(localStorage.getItem("id") || 0 ),
                showAll: true,
            }
        }, 
        computed: {
            filteredTodos() {
                return this.showAll ? this.todos : this.todos.filter((t) => !t.completed)
            },
            incompletedTodos() {
                return this.todos.filter((t) => !t.completed)
            }  
        },

        watch: {
            todos: {
                handler(todos) {
                    localStorage.setItem("todos", JSON.stringify(todos))
                },
                deep: true
            }, 
            id: {
                handler(id) {
                    localStorage.setItem("id", id)
                }
            }
        },

        methods: {
            addTodo(e) {
                const value = e.target.value.trim()
                console.log(value)
                if (value === "") {
                    return;
                }      
                this.todos.push({
                    id: this.id++,
                    task: value,
                    completed: false,
                })
                e.target.value = ''
            }, 
            removeTodo(todo) {
             // Удаляет задачу из массива с индексом to do и 1 объекта
                // Removes a todo from the array at the index of todo and of 1 obj
                this.todos.splice(this.todos.indexOf(todo), 1)
// Сбрасывает счетчик идентификаторов на 0, если массив пуст
                // Resets id counter to 0 if array is empty
                if (this.todos.length === 0) {
                    this.id = 0
                    localStorage.setItem("id", 0)
                }
            }
        }
    }
</script>

<template>
    <div class="menu">
        <p class="tasks-left-text"> {{ incompletedTodos.length }} задание(ий) осталось! </p>
        <button @click="showAll = !showAll" class="determine-todos">
            {{ showAll ? 'Скрыть выполненные задачи' : 'Показать выполненные задачи' }}
        </button>
    </div>



    <input 
        autofocus
        class="add-task"
        placeholder="Добавить новую задачу"
        @keyup.enter="addTodo"
    > 
    <div v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <label class="checkbox-container">
            <input 
                type="checkbox"
                v-model="todo.completed"
                >
            <span class="checkbox"> </span>
        </label>
        <p :class="{ completed: todo.completed }"> {{ todo.task }} </p>
        <button @click="removeTodo(todo)" class="delete-button"> X </button>
    </div> 
</template>



<style scoped>
    .menu {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 8px;
    }

    .tasks-left-text {
        color: white;
        text-align: left;
        margin-bottom: 6px;
    }

    .determine-todos {
        font-family: Verdana, sans-serif;
        align-self: center;
        border: solid grey;
        border-radius: 4px;
        color: white;
        font-weight: bold;
        background-color: rgb(120, 112, 112);
        padding: 4px 8px;
    }

    .add-task {
        padding: 8px;
        font-size: 18px;
        width: 100%;
        background-color: inherit;
        border: 1px solid rgb(23, 19, 19);
        border-radius: 6px;
        color: white;
        margin: 8px 0px;
    }

    .add-task::placeholder {
        font-size: 14px;
    }

    .add-task:focus {
        border: solid white;
        border-width: 1px;
    } 

    .todo-item {
        display: flex;
        justify-content: space-between;
        border: solid rgb(239, 232, 232);
        border-width: 2px;
        background-color: rgb(109, 107, 107);
        padding: 16px;
        color: white;
    }

    /* Customising checkbox guide https://www.w3schools.com/howto/howto_css_custom_checkbox.asp */
    
    /* Customise checkbox container */
    .checkbox-container {
        display: block;
        position: relative;
        padding-left: 35px;
        margin-bottom: 12px;
        cursor: pointer;
        font-size: 22px;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }

    /* Hide default checkbox */
    .checkbox-container input {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
    }
    
    /* Create a custom checkbox */
    .checkbox {
        height: 25px;
        width: 25px;
        background-color: rgb(156, 151, 151);
        position: absolute;
        top: 0;
        left: 0;
    }

    .checkbox-container:hover input ~ .checkbox {
        background-color: rgb(115, 113, 144);
    }

    .checkbox-container input:checked ~ .checkbox {
        background-color:rgb(115, 113, 144);
    }

    .checkbox:after {
        content: "";
        position: absolute;
        display: none;
    }

    /* Show the checkbox when checked */
    .checkbox-container input:checked ~ .checkbox:after {
        display: block;
    }

    /* Style the checkbox */
    .checkbox-container .checkbox:after {
        left: 9px;
        top: 5px;
        width: 5px;
        height: 10px;
        border: solid white;
        border-width: 0 3px 3px 0;
        -webkit-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
        transform: rotate(45deg);
    }
    .completed {
        text-decoration: line-through;
        text-decoration-thickness: 3px;
        color: #ccc;
    }

    .delete-button {
        border: none;
        font-weight: bold;
        background-color: rgb(115, 113, 144);
        color: white;
        font-size: 18px;
    }
</style>