<template>
    <v-app style='margin:30px'>
        <v-card>
        <!--panel header-->
            <v-card-title
                class='headline primary justify-center'
                primary-title style='color: white;'>
                <v-spacer>
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    <span class="fa-solid fa-bars"/> FRAMEWORKS
                </v-spacer>
                <v-btn color="primary" large @click="addTaskForm"> <span class="fa-solid fa-circle-plus"/> ADD </v-btn>
            </v-card-title>
        <!--panel body-->
            <v-simple-table style='margin: 20px'>
                <template v-slot:default>
                <!--table header-->
                    <thead>
                    <tr>
                        <th class="column text-center"> Title </th>
                        <th class="column text-center"> Description </th>
                        <th class="column text-center"> Deadline </th>
                        <th class="column text-center"> Priority </th>
                        <th class="column text-center"> Is Complete </th>
                        <th class="column text-center"> Action </th>
                    </tr>
                    </thead>
                <!--table body-->
                    <tbody>
                        <tr v-for="(task,index) in tasks" :key="index">
                            <td class="text-center">{{task.title}}</td>
                            <td class="text-center">{{task.description}}</td>
                            <td class="text-center">{{task.deadline}}</td>
                            <td class="text-center">{{task.priority}}</td>
                            <td>
                                <v-layout justify-center>
                                    <v-checkbox v-model="tasks[index].isComplete"/>
                                </v-layout>
                            </td>
                            <td>
                                <v-layout justify-center v-if="!tasks[index].isComplete">
                                    <v-btn class="button mt-2" color="primary" large @click="updateTaskForm(index)">
                                        <span class="fa-solid fa-pen-to-square"/>UPDATE
                                    </v-btn>
                                </v-layout>
                                <v-layout justify-center>
                                    <v-btn class="button mb-2" color="error" large @click="deleteTask(index)">
                                        <span class="fa-solid fa-circle-xmark"/>DELETE
                                    </v-btn>
                                </v-layout>
                            </td>
                        </tr>
                    </tbody>
                </template>
            </v-simple-table>
        </v-card>
        <PopUpForm v-show="Visible"
            :Visible=Visible
            :Added=Added
            :pastDescription=description
            :pastdate=deadline
            :pastPriority=priority
            :tasks=tasks
            @addTask="addTask"
            @updateTask="updateTask"
            @close="closeForm"></PopUpForm>
        </v-app>
    </template>
    <script>
    //master
        import PopUpForm from './PopUpForm.vue'
        export default {
            name:'ToDo',
            components:{
                PopUpForm,
            },
        //life cycles
            created(){},
        //methods
            methods: {
                closeForm(){
                    this.Visible=false;
                },
                addTaskForm(){
                    this.Added=true;
                    this.Visible=true;
                },
                addTask(title, description, deadline, priority){
                    this.tasks.push({
                        title:title,
                        description:description,
                        deadline:deadline,
                        priority:priority,
                        isComplete:false,
                    });
                    this.$toasted.success("Task has been Added");
                },
                updateTaskForm(index){
                    this.taskIndex=index;
                    this.Added=false;
                    this.description=this.tasks[this.taskIndex].description;
                    this.deadline=this.tasks[this.taskIndex].deadline;
                    this.priority=this.tasks[this.taskIndex].priority;
                    this.Visible=true;
                },
                updateTask(description, deadline, priority){
                    this.tasks[this.taskIndex].description=description;
                    this.tasks[this.taskIndex].deadline=deadline;
                    this.tasks[this.taskIndex].priority=priority;
                    this.$toasted.success("Task has been updated");
                },
                deleteTask(index){
                    this.tasks.splice(index, 1);
                    this.$toasted.success("Task has been Deleted");
                },
            }, 
        //watchers
            watch: {},
        //computed
            computed: {},
        //global vars
              data(){
                return{
                    Visible:false,
                    Added:true,
                    taskIndex:null,
                    title:'',
                    description:'',
                    deadline:'',
                    priority:'low',
                    tasks:[],
                }
            },
        }
    </script>