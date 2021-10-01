<template>
  <q-page class="bg-grey-3 column">
		<div class="row q-pa-sm bg-primary">
      <q-input 
				filled 
				v-model="newTask" 
				placeholder="Add task" 
				maxlength="22" 
				dense
				bg-color="white"
				color="black"
				class="col"
				@keyup.enter="addTask"
				>

        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask"/>
        </template>
      </q-input>
		</div>
    <q-list 
			class="bg-white"
			separator
			bordered>
      <!--
        Rendering a <label> tag (notice tag="label")
        so QCheckboxes will respond to clicks on QItems to
        change Toggle state.
      -->

      <q-item 
				v-for="(task, index) in tasks"
				v-ripple
				:key="index"
				clickable
				@click="task.done = !task.done"
				:class="{ 'done' : task.done }"
				>
        <q-item-section avatar>
          <q-checkbox 
						v-model="task.done"
						class="no-pointer-events"
						color="primary"
						/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
				<q-item-section
					v-if="task.done"
					side>
           <q-btn 
						flat 
						round 
						dense
						@click.stop="deleteTask(index)"
						color="primary" 
						icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
		<div v-if="tasks.length === 0" class="no-tasks absolute-center">
			<q-icon
				name="check"
				size="100px"
				color="primary" />
			<div class="text-h5 text-primary text-center">
				No Tasks
			</div>
		</div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Todo',
	data() {
		return {
			tasks: [
				{
					title: 'Get frui',
					done: false,
				},
				{
					title: 'lmao',
					done: false,
				},
				{
					title: 'gg',
					done: false,
				},
			],
			newTask: "",
		}
	},
	methods: {
		deleteTask(index) {
			this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
				this.$q.notify('Task deleted')
      })
		},
		addTask() {
			if (this.newTask !== "")
				this.tasks.push({title: this.newTask, done: false})
			this.newTask = ""
		}
	}
})
</script>

<style scoped lang="scss">
	.done{
		.q-item__label{
			text-decoration: line-through;
			color: #bbb;
		}
	}
	.no-tasks {
		opacity: 0.5;
	}
</style>