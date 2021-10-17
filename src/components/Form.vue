<template>
	<div class="form-wrapper">
		<div v-if="formOn" class="form-wrapper__inner">
			<form @submit.prevent="newElem" class="form" >
				<textarea class="form__input" placeholder="Введите название" v-model="title"></textarea>
				<button class="button add-button">
					<span class="button__icon">
						<svg class="button__svg button__svg_big button__svg_dark"
								xmlns="http://www.w3.org/2000/svg" 
								viewBox="0 0 24 24" 
						>
							<path d="M0 0h24v24H0z" fill="none"/>
							<path d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"/>
						</svg>
					</span>
				</button>          
			</form>
			<a href="#" class="close-form-switch form-switch_purple" @click="toggleForm">Отмена</a>
		</div>
		<a href="#" class="open-form-switch form-switch_purple" v-else @click="toggleForm">{{ msg }}</a>
	</div>
</template>

<script>
import { ref } from 'vue' 

export default {
	props: {
		array: Array,
		msg: String
	},
	setup(props , { emit }) {
		const title = ref('')

		const formOn = ref(false)

		const toggleForm = () => formOn.value = !formOn.value

		const newElem = () => {
				emit('createitem', props.array, title.value)
			
			title.value = ''
		}
   
	 return {
		 title,
		 formOn,
		 newElem,
		 toggleForm
	 }
	}
}
</script>

<style>

</style>