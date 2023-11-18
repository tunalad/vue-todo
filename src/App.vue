<script setup>
import { ref, onMounted } from "vue";

const todoList = ref([]);
const inputField = ref("");

function addTodo(todo) {
    if (todo.trim(" ") === "") return;

    todoList.value.push({
        id: new Date().getTime(),
        description: todo,
        completed: false,
    });
    inputField.value = "";
    updateList();
}

function removeTodo(todo) {
    todoList.value = todoList.value.filter((t) => t.id != todo);
    updateList();
}

function clearTodos() {
    let completedTodos = todoList.value.filter((t) => t.completed);

    for (let t in completedTodos) removeTodo(completedTodos[t].id);
    updateList();
}

function updateList() {
    localStorage.setItem("todoList", JSON.stringify(todoList.value));
}

onMounted(() => {
    todoList.value = JSON.parse(localStorage.getItem("todoList")) || [];
});
</script>

<template>
    <div style="display: flex; justify-content: center">
        <img src="./assets/vue.svg" alt="vue-logo" style="padding: 0 1rem" />
        <h1>vue-todo</h1>
        <img src="./assets/vue.svg" alt="vue-logo" style="padding: 0 1rem" />
    </div>

    <form action="submit">
        <input
            type="text"
            placeholder="e.g. Do the Laundry"
            v-model="inputField"
        />
        <button @click.prevent="addTodo(inputField)">Submit</button>
    </form>

    <a
        href="#"
        @click.prevent="clearTodos"
        v-if="todoList.filter((t) => t.completed).length > 0"
    >
        Clear {{ todoList.filter((t) => t.completed).length }} completed task(s)
    </a>

    <br />

    <ul>
        <li v-for="i in todoList">
            <input type="checkbox" v-model="i.completed" />

            <p :class="i.completed ? 'completed' : ''">
                {{ i.description }}
            </p>

            <button class="btn-delete" @click="removeTodo(i.id)">Delete</button>
        </li>
    </ul>
</template>

<style scope>
body {
    min-height: 0;
}

p {
    display: inline;
    line-height: 2rem;
    padding: 0 1rem;
}

input[type="checkbox"] {
    transform: scale(1.25);
    accent-color: rgba(0, 128, 0, 0.75);
}

ul {
    margin: 0 auto;
    padding: 0 1rem;

    display: block;
    text-align: left;
    list-style-type: "- ";
    list-style-type: none;
    max-width: 30rem;
    min-width: 30rem;
}

button {
    padding: 0.25rem 0.5rem;
    margin: 0 0.5rem;
}

.completed {
    text-decoration: line-through;
    color: rgba(77, 216, 77, 0.75);
}

.btn-delete {
    float: right;
    background-color: rgba(224, 55, 55, 0.6);
}

#edit {
    background-color: rgba(113, 46, 179, 0.6);
}
</style>
