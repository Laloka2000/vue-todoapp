<template>
    <div class="todo-container">
        <h1>Todo App</h1>
        <div class="input-container">
            <input v-model="newTodo" @keyup.enter="addTodo" type="text" placeholder="Add a new todo" />
            <select v-model="newCategory">
                <option value="">Select Category</option>
                <option value="Work">Work</option>
                <option value="Personal">Personal</option>
                <option value="Urgent">Urgent</option>
            </select>
            <!-- Priority Selector -->
            <select v-model="newPriority">
                <option value="">Select Priority</option>
                <option value="High">High</option>
                <option value="Medium">Medium</option>
                <option value="Low">Low</option>
            </select>
            <button @click="addTodo" class="add-button">Add</button>
        </div>

        <!-- Search and Filter Options -->
        <div class="filter-container">
            <input v-model="searchQuery" type="text" placeholder="Search todos..." />
            <select v-model="filterStatus">
                <option value="all">All</option>
                <option value="completed">Completed</option>
                <option value="pending">Pending</option>
            </select>
        </div>

        <transition-group name="fade" tag="ul" class="todo-list">
            <li v-for="(todo, index) in filteredTodos" :key="todo.id" class="todo-item" :class="todo.priority">
                <div class="todo-content" @click="toggleComplete(index)">
                    <span :class="{ 'completed': todo.completed }">{{ todo.text }}</span>
                    <span class="category">({{ todo.category }})</span>
                    <span class="priority">[{{ todo.priority }}]</span>
                </div>
                <button @click="startEditing(index)" class="edit-button">Edit</button>
                <button @click="deleteTodo(index)" class="delete-button">Delete</button>
            </li>
        </transition-group>
        <div v-if="isEditing" class="edit-form">
            <h3>Edit Todo</h3>
            <input v-model="editingText" type="text" />
            <select v-model="editingCategory">
                <option value="">Select Category</option>
                <option value="Work">Work</option>
                <option value="Personal">Personal</option>
                <option value="Urgent">Urgent</option>
            </select>
            <!-- Edit Priority Selector -->
            <select v-model="editingPriority">
                <option value="">Select Priority</option>
                <option value="High">High</option>
                <option value="Medium">Medium</option>
                <option value="Low">Low</option>
            </select>
            <button @click="saveEdit" class="save-button">Save</button>
            <button @click="cancelEdit" class="cancel-button">Cancel</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoApp',
    data()
    {
        return {
            newTodo: '',
            newCategory: '',
            newPriority: '', 
            todos: JSON.parse(localStorage.getItem('todos')) || [],
            isEditing: false,
            editingIndex: null,
            editingText: '',
            editingCategory: '',
            editingPriority: '',
            searchQuery: '',
            filterStatus: 'all'
        };
    },
    watch: {
        todos: {
            handler(newTodos){
                localStorage.setItem('todos', JSON.stringify(newTodos));
            },
            deep: true
        }
    },
    computed: {
        filteredTodos()
        {
            return this.todos.filter(todo =>
            {
                const matchesSearch = todo.text.toLowerCase().includes(this.searchQuery.toLowerCase());
                const matchesFilter =
                    this.filterStatus === 'all' ||
                    (this.filterStatus === 'completed' && todo.completed) ||
                    (this.filterStatus === 'pending' && !todo.completed);
                return matchesSearch && matchesFilter;
            });
        }
    },
    methods: {
        addTodo()
        {
            if (this.newTodo.trim() !== '' && this.newCategory !== '' && this.newPriority !== '')
            {
                this.todos.push({
                    id: Date.now(),
                    text: this.newTodo,
                    category: this.newCategory,
                    priority: this.newPriority, 
                    completed: false
                });
                this.newTodo = '';
                this.newCategory = '';
                this.newPriority = ''; 
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
        startEditing(index)
        {
            this.isEditing = true;
            this.editingIndex = index;
            this.editingText = this.todos[index].text;
            this.editingCategory = this.todos[index].category;
            this.editingPriority = this.todos[index].priority; // Set priority for editing
        },
        saveEdit()
        {
            if (this.editingText.trim() !== '' && this.editingCategory !== '' && this.editingPriority !== '')
            {
                this.todos[this.editingIndex].text = this.editingText.trim();
                this.todos[this.editingIndex].category = this.editingCategory;
                this.todos[this.editingIndex].priority = this.editingPriority; // Save priority for edited todo
                this.isEditing = false;
                this.editingIndex = null;
                this.editingText = '';
                this.editingCategory = '';
                this.editingPriority = ''; // Reset editing priority
            }
        },
        cancelEdit()
        {
            this.isEditing = false;
            this.editingIndex = null;
            this.editingText = '';
            this.editingCategory = '';
            this.editingPriority = ''; // Reset editing priority
        }
    }
};
</script>

<style scoped>
.todo-container {
    max-width: 100%;
    margin: 20px;
    padding: 20px;
    text-align: center;
    font-family: 'Poppins', sans-serif;
    background-color: #f9f9f9;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.input-container,
.filter-container {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;
}

@media (min-width: 768px) {

    .input-container,
    .filter-container {
        flex-direction: row;
        justify-content: center;
    }
}

input[type="text"] {
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 8px;
    margin-right: 10px;
    flex: 1;
}

select {
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 8px;
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
    margin-top: 10px;
}

.add-button:hover {
    background-color: #369b74;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}

.todo-list {
    list-style-type: none;
    padding: 0;
}

.todo-item {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: #ffffff;
    padding: 15px;
    margin: 10px 0;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s;
}

@media (min-width: 768px) {
    .todo-item {
        flex-direction: row;
    }
}

.todo-item:hover {
    transform: scale(1.02);
}

.todo-content {
    flex: 1;
    cursor: pointer;
}

.completed {
    text-decoration: line-through;
    color: #888;
}

.category {
    font-style: italic;
    color: #555;
    margin-left: 10px;
}

.edit-button {
    background-color: #f1c40f;
    color: white;
    border: none;
    border-radius: 8px;
    padding: 10px;
    margin-top: 10px;
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
    margin-top: 10px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.delete-button:hover {
    background-color: #c0392b;
}

.edit-form {
    margin-top: 20px;
    background-color: #ffffff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    transition: opacity 0.5s;
}

.save-button {
    padding: 10px;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.save-button:hover {
    background-color: #369b74;
}

.cancel-button {
    padding: 10px;
    background-color: #e74c3c;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    margin-left: 10px;
}

.cancel-button:hover {
    background-color: #c0392b;
}
</style>