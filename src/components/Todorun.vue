<template>
    <div class="container" style="max-width: 600px">
        <!-- Heading -->
        <h2 class="text-center mt-5">MY TODO APP</h2>

        <!-- Input -->
        <div class="d-flex mt-5 box-model">
            <input type="text" v-model="task" placeholder="Enter task" class="w-100 form-control box-model-input" />
            <button class="btn btn-danger rounded-0 text-white" @click="submitTask">
                SUBMIT
            </button>
        </div>

        <!-- Task table -->
        <table class="table table-bordered mt-5 box-model">
            <thead>
                <tr>
                    <th scope="col">Task</th>
                    <th scope="col" style="width: 120px">Status</th>
                    <th scope="col" class="text-center">#</th>
                    <th scope="col" class="text-center">#</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td>
                        <span :class="{ 'line-through': task.status === 'finished' }">
                            {{ task.name }}
                        </span>
                    </td>
                    <td>
                        <span class="pointer noselect" @click="changeStatus(index)" :class="{
                            'text-danger': task.status === 'to-do',
                            'text-success': task.status === 'finished',
                            'text-warning': task.status === 'in-progress',
                        }">
                            {{ capitalizeFirstChar(task.status) }}
                        </span>
                    </td>
                    <td class="text-center">
                        <div @click="deleteTask(index)">
                            <span class="fa fa-trash pointer"></span>
                        </div>
                    </td>
                    <td class="text-center">
                        <div @click="editTask(index)">
                            <p class="fa fa-pen pointer"></p>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
        <div class="text-center">
            Made with <i class="fa fa-heart text-danger" aria-hidden="true"></i> by Sunita Shrestha
        </div>
    </div>
</template>

<script>
export default {
    name: "HelloWorld",
    props: {
        msg: String,
    },
    data() {
        return {
            task: "",
            editedTask: null,
            statuses: ["to-do", "in-progress", "finished"],
            /* Status could be: 'to-do' / 'in-progress' / 'finished' */
            tasks: [
                {
                    name: "Steal bananas from the supermarket.",
                    status: "to-do",
                },
                {
                    name: "Eat 1 kg chocolate in 1 hour.",
                    status: "in-progress",
                },
                {
                    name: "Create YouTube video.",
                    status: "finished",
                },
            ],
        };
    },
    methods: {
        /**
         * Capitalize first character
         */
        capitalizeFirstChar(str) {
            return str.charAt(0).toUpperCase() + str.slice(1);
        },
        /**
         * Change status of task by index
         */
        changeStatus(index) {
            let newIndex = this.statuses.indexOf(this.tasks[index].status);
            if (++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.statuses[newIndex];
        },
        /**
         * Deletes task by index
         */
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },
        /**
         * Edit task
         */
        editTask(index) {
            this.task = this.tasks[index].name;
            this.editedTask = index;
        },
        /**
         * Add / Update task
         */
        submitTask() {
            if (this.task.length === 0) return;
            /* We need to update the task */
            if (this.editedTask != null) {
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
            } else {
                /* We need to add new task */
                this.tasks.push({
                    name: this.task,
                    status: "todo",
                });
            }
            this.task = "";
        },
    },
};
</script>

<style scoped>
.pointer {
    cursor: pointer;
}

.noselect {
    -webkit-touch-callout: none;
    /* iOS Safari */
    -webkit-user-select: none;
    /* Safari */
    -khtml-user-select: none;
    /* Konqueror HTML */
    -moz-user-select: none;
    /* Old versions of Firefox */
    -ms-user-select: none;
    /* Internet Explorer/Edge */
    user-select: none;
    /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}

.line-through {
    text-decoration: line-through;
}
.box-model{
    box-shadow: 0 10px 30px -10px rgba(2,12,27,0.7);
    
}
.box-model-input{
    outline: none;
    border-radius: none;
    border: none;
}
</style>