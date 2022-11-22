<template>
    <div class="modal-backdrop">
        <div class="modal">
            <v-card>
                <v-card-title class='headline primary justify-center'
                    primary-title style='color: white'>
                    <div v-if="Added" key="add">
                        <span class="fa-solid fa-circle-plus"/> Add Task
                    </div>
                    <div v-else key="update">
                        <span class="fa-solid fa-pen-to-square"/> Update Task
                    </div>
                </v-card-title>
                <v-form ref="form" lazy-validation>
                    <v-layout v-if="Added" justify-center class="elements mx-5 mb-0 mt-7">
                        <v-text-field label="Title" outlined v-model="title" 
                            :rules="titleRules" required />
                    </v-layout>
                    <v-layout justify-center class="elements mx-5 my-2">
                        <v-text-field label="Description" outlined v-model="description"
                            :rules="[v => !!v || 'Description is required']" required />
                    </v-layout>
                    <v-layout justify-center class="elements mx-5 my-0">
                    <v-menu
                        :close-on-content-click="false"
                        transition="scale-transition"
                        offset-y
                        max-width="290px"
                        min-width="auto"
                        >
                    <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                        v-model="computedDateFormatted"
                        label="Deadline"
                        append-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                        outlined
                        ></v-text-field>
                    </template>
                    <v-date-picker
                        v-model="date"
                        no-title
                        @input="menu2 = false"
                    ></v-date-picker>
                    </v-menu>
                    </v-layout>
                    <v-radio-group label="Priority" class="elements mx-5 my-0" v-model="priority">
                        <v-layout align-start row d-flex justify-space-between>
                            <v-radio label="Low" value="low"/>
                            <v-radio label="Med" value="med"/>
                            <v-radio label="High" value="high"/>
                        </v-layout>
                    </v-radio-group>
                    <v-layout justify-end class="elements mx-2 mt-0 mb-2">
                        <div v-if="Added" key="add1">
                            <v-btn color="primary" elevation="2" @click="addTask" class="mr-2">
                                <i class="fa-solid fa-circle-plus"></i> ADD
                            </v-btn>
                        </div>
                        <div v-else key="update1">
                            <v-btn color="primary" elevation="2" @click="updateTask" class="mr-2">
                                <i class="fa-solid fa-pen-to-square"></i> EDIT
                            </v-btn>
                        </div>
                        <v-btn color="error" elevation="2" @click="close">
                            <i class="fa-solid fa-ban"></i> CANCEL
                        </v-btn>
                    </v-layout>
                </v-form>
            </v-card>
        </div>
    </div>
</template>

<script>
export default{
    name:'PopUpForm',
    props:{
        Visible:Boolean,
        Added:Boolean,
        pastDescription:String,
        pastdate:String,
        pastPriority:String,
        tasks:Array,
    },
    watch:{
        Visible(newVal){
            this.$refs.form.resetValidation();
            if(newVal && !this.Added){
                this.getParentData();
            }
        },
        date () {
            this.dateFormatted = this.formatDate(this.date)
        },
    },
    computed:{
        computedDateFormatted () {
            return this.formatDate(this.date)
        },
    },
    methods:{
        formatDate (date) {
            if (!date) return null
            const [year, month, day] = date.split('-')
            return `${month}/${day}/${year}`
        },
        parseDate (date) {
            if (!date) return null
            const [month, day, year] = date.split('/')
            return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
        },
        addTask(){
            if(this.$refs.form.validate()){
                this.$emit("addTask", this.title, this.description, this.formatDate(this.date), this.priority);
                this.close();
            }
        },
        updateTask(){
            if(this.$refs.form.validate()){
                this.$emit("updateTask", this.description, this.formatDate(this.date), this.priority);
                this.close();
            }
        },
        close(){
            this.title='';
            this.description='';
            this.date='';
            this.priority='low';
            this.$refs.form.resetValidation();
            this.$emit('close');
        },
        getParentData(){
            this.description=this.pastDescription;
            this.date=this.parseDate(this.pastdate);
            this.priority=this.pastPriority;
            console.log(this.description,this.pastdate,this.pastPriority)
        },
        checkValidTitle(title){
            return !this.tasks.filter(e=>e.title===title).length>0
        }
    },
    data(){
        return{
            title: '',
            description: '',
            priority: 'low',
            date: '',
            titleRules: [
                v=>!!v||'Title is required',
                v=>this.checkValidTitle(v)||'Title must be Unique',
            ],
        }
    }
};
</script>
<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, .5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }
</style>