<template>
    <div class="todo-container">
        <h1>Todo App</h1>
        <div class="input-container">
            <input v-model="newTodo" @keyup.enter="addTodo" type="text" placeholder="Add a new todo" />
            <button @click="addTodo" class="add-button">Add</button>
        </div>
        <ul class="todo-list">
            <li v-for="(todo, index) in todos" :key="index" class="todo-item">
                <div class="todo-content" @click="toggleComplete(index)">
                    <span :class="{ 'completed': todo.completed }">{{ todo.text }}</span>
                </div>
                <button @click="editTodo(index)" class="edit-button">Edit</button>
                <button @click="deleteTodo(index)" class="delete-button">Delete</button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'TodoApp',
    data()
    {
        return {
            newTodo: '',
            todos: []
        };
    },
    methods: {
        addTodo()
        {
            if (this.newTodo.trim() !== '')
            {
                this.todos.push({
                    text: this.newTodo,
                    completed: false
                });
                this.newTodo = '';
            }
        },
        deleteTodo(index)
        {
            this.todos.splice(index, 1);
        },
        toggleComplete(index)
        {
            this.todos[index].completed = !this.todos[index].completed;
        },
        editTodo(index)
        {
            const updatedText = prompt('Edit your todo:', this.todos[index].text);
            if (updatedText !== null && updatedText.trim() !== '')
            {
                this.todos[index].text = updatedText.trim();
            }
        }
    }
};
</script>

<style scoped>
.todo-container {
    max-width: 600px;
    margin: 50px auto;
    text-align: center;
    font-family: 'Poppins', sans-serif;
    background-color: #f9f9f9;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.input-container {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

input[type="text"] {
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 8px;
    flex: 1;
    margin-right: 10px;
}

.add-button {
    padding: 12px;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.add-button:hover {
    background-color: #369b74;
}

.todo-list {
    list-style-type: none;
    padding: 0;
}

.todo-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #ffffff;
    padding: 15px;
    margin: 10px 0;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.todo-content {
    flex: 1;
    cursor: pointer;
}

.completed {
    text-decoration: line-through;
    color: #888;
}

.edit-button {
    background-color: #f1c40f;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 10px;
    margin-right: 10px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.edit-button:hover {
    background-color: #d4ac0d;
}

.delete-button {
    background-color: #e74c3c;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 10px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.delete-button:hover {
    background-color: #c0392b;
}
</style>