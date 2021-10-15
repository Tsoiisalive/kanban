<script>
  import { ref } from 'vue'
  import TodoList from './TodoList.vue'
  import Form from './Form.vue'
  import Modal from './Modal.vue'

	export default {
    components: {
      TodoList,
      Form,
      Modal
    },
		setup() {
			const items = ref([
				{ id: 0,
          title: 'Task 1',
          list: 'Todo',
          todoListOn: true,
          isModalOpen: false,
          tasks: [
            {
            "title": "do something",
            "completed": false,
            },       
            {
            "title": "do something else",
            "completed": false,
            },   
            {
            "title": "do something else 2",
            "completed": false,
            },   
          ]
        },
				{ id: 1,
          title: 'Task 2',
          list: 'Todo',
          todoListOn: false,
          isModalOpen: false,
          tasks: [
            {
            "title": "do something",
            "completed": false,
            },       
            {
            "title": "do something else",
            "completed": false,
            },   
            {
            "title": "do something else 2",
            "completed": false,
            },   
          ]
        }, 
				{ id: 2,
          title: 'Task 3',
          list: 'Done',
          todoListOn: false,
          isModalOpen: false,
          tasks: [
            {
            "title": "do something",
            "completed": false,
            },       
            {
            "title": "do something else",
            "completed": false,
            },   
            {
            "title": "do something else 2",
            "completed": false,
            },   
          ]
        },               
			])

      const lists = ref(['Todo','In progress', 'Done'])

			const getList = (list) => {
				return items.value.filter((item) => item.list == list)
			}

			const startDrag = (event, item) => {
				event.dataTransfer.dropEffect = 'move'
				event.dataTransfer.effectAllowed = 'move'
				event.dataTransfer.setData('itemId', item.id)
			}

			const onDrop = (event, list) => {
				const itemId = event.dataTransfer.getData('itemId')
				const item = items.value.find((item) => item.id == itemId)
				item.list = list 
        
			}

      const deleteElement = (arr, element) => {
        const index = arr.indexOf(element)
        arr.splice(index, 1)
      }

      const deleteList = (element) => {
        let arrOnDelete = getList(element) 
        arrOnDelete.forEach(item => {
          deleteElement(items.value, item)
        })
        
        deleteElement(lists.value, element)
      }

      const addItem = (arr, element) => {
        if(element) {
          arr.push(element)
        }
      }

      const createTask = (arr, value, list) => {
        let task = {
          id: items.value.length,
          title: value,
          list: list,
          todoListOn: true,
          isModalOpen: false,
          tasks: []
        }
        addItem(arr, task)
      }

      const toggleList = (item) => {
        item.todoListOn = !item.todoListOn
      }

      const toggleModal = (item) => {
        item.isModalOpen = !item.isModalOpen
      }

			return {
        lists,
        items,
				getList,
				startDrag,
				onDrop,
        deleteElement,
        deleteList,
        addItem,
        toggleList,
        createTask,
        toggleModal,
			}
		},
	}
</script>

<template>
  <div class="desk desk__wrapper">
    <h1 class="desk__heading">Tasktracker</h1>
    <div class="desk__drop-zone drop-zone drop-zone_lightgray" 
			v-for="(list, i) in lists" 
			:key="i"
			@drop="onDrop($event, list)"
			@dragenter.prevent
			@dragover.prevent
    >
      <div class="drop-zone__header">
        <h2>{{ list }}</h2>
        <button class="button delete-button" @click="deleteList(list)">
          <span class="button__icon">
            <svg class="button__svg button__svg_small button__svg_dark"
                xmlns="http://www.w3.org/2000/svg" 
                  viewBox="0 0 24 24" 
                  fill="#000000"
            >
              <path d="M0 0h24v24H0z" fill="none"/>
              <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
            </svg>
          </span>
        </button>  
      </div>        
      <div class="drop-zone__content">
        <div v-for="item in getList(list)" 
          :key="item.id" 
          class="drop-zone__element task"
          draggable="true"
          @dragstart="startDrag($event, item)"
        >
        <Modal v-if="item.isModalOpen" class="modal"> 
          <div class="task__header task__header_modal">
            <button class="button close-modal-button" @click="toggleModal(item)">
              <span class="button__icon">
                <svg class="button__svg button__svg_small button__svg_light"
                     xmlns="http://www.w3.org/2000/svg"
                     height="24px" viewBox="0 0 24 24" 
                     width="24px" fill="#000000"
                 >
                  <path d="M0 0h24v24H0V0z" fill="none"/>
                  <path d="M5 16h3v3h2v-5H5v2zm3-8H5v2h5V5H8v3zm6 11h2v-3h3v-2h-5v5zm2-11V5h-2v5h5V8h-3z"/>
                </svg>
              </span>
            </button>           
          </div>
          <div class="task__content task__content_modal">
            <h3 class="task__title task__title_modal">{{ item.title }}</h3>
            <TodoList class="todo_modal" :todoList="item.tasks" @newTask="addItem"/>          
          </div>
        </Modal>
          <div class="task__header">
            <button class="button open-modal-button" @click="toggleModal(item)">
              <span class="button__icon">
                <svg class="button__svg button__svg_small button__svg_light" 
                    xmlns="http://www.w3.org/2000/svg" 
                    height="24px" 
                    viewBox="0 0 24 24" 
                    width="24px" 
                    fill="#000000"
                >
                  <path d="M0 0h24v24H0z" fill="none"/>
                  <path d="M7 14H5v5h5v-2H7v-3zm-2-4h2V7h3V5H5v5zm12 7h-3v2h5v-5h-2v3zM14 5v2h3v3h2V5h-5z"/>
                </svg>
              </span>
            </button>     
            <button class="button delete-button " @click="deleteElement(items, item)">
              <span class="button__icon">
                <svg class="button__svg button__svg_small button__svg_light"
                    xmlns="http://www.w3.org/2000/svg" 
                      viewBox="0 0 24 24" 
                      fill="#000000"
                >
                  <path d="M0 0h24v24H0z" fill="none"/>
                  <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
                </svg>
              </span>
            </button>          
          </div>
          <div class="task__content">
            <h3 class="task__title" @click="toggleList(item)">{{ item.title }}</h3>
            <TodoList :todoList="item.tasks" v-if="item.todoListOn" @newTask="addItem"/>          
          </div>
        </div>        
      </div>
      <div class="drop-zone__footer">
        <Form :array="items" :listName="list" msg="Добавить задачу" @createtask="createTask" class="drop-zone__new-task-form"/>
      </div>
    </div>
    <Form msg="Добавить категорию" :array="lists" @createitem="addItem" class="desk__form drop-zone drop-zone_lightgray"/>
  </div>
</template>

<style lang="scss"> 
</style>
