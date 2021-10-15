<template>
	<div class="todo">
		<div class="todo__wrapper">
			<div class="todo__list">
				<ul class="todo__items">
					<li 
							v-for="(task, i) in todoList" 
							:key="i" 
							class="todo__item" 
							@click="toggleTask(task)"
							:class="{ 'todo__item_completed': task.completed }"
					>
							{{task.title}}
					</li>
				</ul>
			</div>    
			<Form :array="todoList" msg="Добавьте подзадачу" class="todo__form" @createitem="addTask" />
		</div>
	</div>
</template>

<script>
import { ref } from 'vue'
import Form from './Form.vue'
export default {
	components: {
			Form,
	},
	props: {
					todoList: Array
			},
	setup(props, { emit }) {
		const taskTitle =  ref('')

		const addTask = (arr, value) => {
				const task = {
						title: value,
						completed: false
				}
				
				emit('newTask', arr, task)
		}

		const toggleTask = (task) => {
				task.completed = !task.completed
		}


		return {
				taskTitle,
				addTask,
				toggleTask
		}
	}
}
</script>

<style lang="scss">
</style>