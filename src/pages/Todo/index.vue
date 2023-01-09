<template>
	<!-- <q-page class="q-pa-lg">
    <h5 class="q-mt">Todo</h5>
    </q-page> -->
	<q-page class="bg-grey-3 column">
		<div class="row q-pa-sm bg-primary">
			<q-input
				filled
				bottom-slots
				bg-color="white"
                class="col"
                square
				dense
                placeholder="Add Task" 
                v-model="newTask"
                @keyup.enter="addTask"
			>
				<template v-slot:append>
					<q-btn round dense flat icon="add" @click="addTask" />
				</template>
			</q-input>
		</div>
		<q-list class="bg-white" separator bordered>
			<q-item
				v-for="(task) in tasks"
				@click="task.done = !task.done"
				:key="task.id"
				:class="{ 'done bg-blue-1': task.done }"
				clickable
				v-ripple
			>
				<q-item-section avatar>
					<q-checkbox v-model="task.done" val="teal" color="primary" />
				</q-item-section>
				<q-item-section>
					<q-item-label>{{ task.title }}</q-item-label>
				</q-item-section>
                <q-item-section v-if="!task.done" side>
                <q-btn @click.stop="updateTask(task)" dense flat icon="edit" color="primary" />
                </q-item-section>
               
				<q-item-section v-if="task.done" side>
					<q-btn @click.stop="deleteTask(task.id)" dense flat icon="delete" color="red" />
				</q-item-section>
			</q-item>
		</q-list>
	</q-page>
</template>

<script>
import { ref } from 'vue';
import { useQuasar } from 'quasar';
export default {
	name: 'TodoView',

	setup() {
        const newTask = ref("");
        const tasks = ref([
			{
                id: "1bb04dcc-a3d6-4ab4-b3f9-1eae704f42ff",
				title: 'Get Bananas',
				done: false,
			},
		]);
		const $q = useQuasar();

        function uuidv4() {
            return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
                (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
            );
        }


		const deleteTask = (id) => {
			$q.dialog({
				title: 'Confirm',
				message: 'are you sure want delete this data ?',
				cancel: true,
				persistent: true,
			}).onOk(() => {
				let filtered = tasks.value.filter((item) => item.id !== id);
                console.log("filtered", filtered)
                tasks.value = filtered
				$q.notify('Task Deleted');
			});
		};

        
        const updateTask = (task) => {
            $q.dialog({
                title: 'Prompt',
                message: 'What is your name?',
                prompt: {
                    model: '',
                    type: 'text' // optional
                },
                cancel: true,
                persistent: true
            }).onOk(data => {
                let updateData = tasks.value.map(item => {
                    if(item.id == task.id){
                        return {
                            ...item,
                            title : data
                        }
                    }
                    else {
                        return {...item}
                    }
                })
                tasks.value = updateData;
            })
        };

        const addTask = () => {
            if (newTask.value.trim() === "") return;
            tasks.value.push({
                id: uuidv4(),
                title: newTask.value,
                done: false,
            });
            newTask.value = "";
        };


        return { tasks, deleteTask, newTask, addTask, updateTask };
	},
};
</script>

<style lang="scss">
.done {
	.q-item__label {
		text-decoration: line-through;
		color: #bbb;
	}
}
</style>
