<template>
	<main>
		<div class="todo">
			<div class="todo-inner">
				<h1>Todo App</h1>

				<form @submit.prevent="addTodo">
					<label>New Todo</label>
					<input type="text" v-model="newTodo" />
					<button class="add" type="submit">Add new todo</button>
				</form>
				<h2>Todo List</h2>

				<ul>
					<li v-for="(todo, index) in todos" :key="index">
						<span :class="{ done: todo.done }" >
							{{ todo.content }}
						</span>
            <div class="actions">
              <button class="delete wh-30" :class='{green: !todo.done, red: todo.done}' @click="doneTodo(todo)">✓</button>
						<button class="delete"  @click="removeTodo(index)">Remove</button>
            </div>

					</li>
				</ul>
        <h4 v-if="!todos.length">Empty list</h4>
			</div>
		</div>
	</main>
</template>
<script>
import { ref } from 'vue'
export default {
	setup() {
		// data
		const newTodo = ref('')
		const todosData = JSON.parse(localStorage.getItem('todos')) || []
		const todos = ref(todosData)
		// methods

		const addTodo = () => {
			if (newTodo.value) {
				todos.value.push({
					done: false,
					content: newTodo.value,
				})
				newTodo.value = ''
        saveData()
			}
		}

		const doneTodo = todo => {
			todo.done = !todo.done
    saveData()

		}

		const removeTodo = index => {
			todos.value.splice(index, 1)
      saveData()
		}

    const saveData = () => {
      const storageData = JSON.stringify(todos.value)
      localStorage.setItem('todos', storageData)
    }

		return {  
			newTodo,
			addTodo,
			todos,
			doneTodo,
			removeTodo,

		}
	},
}
</script>
<style></style>
