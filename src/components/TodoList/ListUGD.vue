<template>
    <v-main class="list">
        <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>
        
        <v-card>
            <v-card-title>
                <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search"
                    single-line
                    hide-details
                ></v-text-field>
                <v-spacer></v-spacer>
                <v-btn color="success" dark @click="dialog = true">
                    Tambah
                </v-btn>
            </v-card-title>
            <v-data-table :headers="headers" :items="todos" :search="search">
                <template v-slot:[`item.priority`]="{ item }">
                    <v-chip
                        :color="getColor(item.priority)"
                        dark
                        >
                        {{ item.priority }}
                    </v-chip>
                </template>


                <template v-slot:[`item.actions`]="{ item }">
                    
                    <v-btn small class="mr-2" @click="editItem(item) ">
                        edit
                    </v-btn>
                    <v-btn small @click="deleteItem(item)">
                        delete
                    </v-btn>
                </template>
            </v-data-table>
        </v-card>

        <v-dialog v-model="dialog" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">Form Todo</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-text-field
                            v-model="formTodo.task"
                            label="Task"
                            required
                        ></v-text-field>
                        
                        <v-select
                            v-model="formTodo.priority"
                            :items="['Penting', 'Biasa', 'Tidak penting']"
                            label="Priority"
                            required
                        ></v-select>
 
                        <v-textarea
                            v-model="formTodo.note"
                            label="Note"
                            required
                        ></v-textarea>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="cancel">
                        Cancel
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="save">
                        Save
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>


        <v-dialog v-model="dialogedit" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">Form Todo</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-text-field
                            v-model="formTodo.task"
                            label="Task"
                            required
                        ></v-text-field>
                        
                        <v-select
                            v-model="formTodo.priority"
                            :items="['Penting', 'Biasa', 'Tidak penting']"
                            label="Priority"
                            required
                        ></v-select>
 
                        <v-textarea
                            v-model="formTodo.note"
                            label="Note"
                            required
                        ></v-textarea>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="cancel">
                        Cancel
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="updateItem">
                        Edit
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>


        <v-dialog v-model="dialogdelete" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">Yakin ingin menghapus?</span>
                </v-card-title>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="green" text @click="deletetidak">
                        TIDAK
                    </v-btn>
                    <v-btn color="red" text @click="deleteYa">
                        YA
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

    </v-main>
</template>
<script>
export default {
    name: "List",
data() {
    return {
        search: null,
        dialog: false,
        dialogdelete: false,
        dialogedit: false,
        sementara: null,
        headers: [
            {
                text: "Task",
                align: "start",
                sortable: true,
                value: "task",
            },
            { text: "Priority", value: "priority" },
            { text: "Note", value: "note" },
            { text: "Actions", value: "actions" },
        ],
        todos: [
            {
                task: "bernafas",
                priority: "Penting",
                note: "huffttt",
            },
            {
                task: "nongkrong",
                priority: "Tidak penting",
                note: "bersama tman2",
            },
            {
                task: "masak",
                priority: "Biasa",
                note: "masak air 500ml",
            },
        ],
        formTodo: {
            task: null,
            priority: null,
            note: null,
        },
    };
},
    methods: {
        save() {
            this.todos.push(this.formTodo);
            this.resetForm();
            this.dialog = false;
        },
        cancel() {
            this.resetForm();
            this.dialog = false;
        },
        resetForm() {
            this.formTodo = {
            task: null,
            priority: null,
            note: null,
            };
        },

        deleteItem(item){
            this.dialogdelete = true;
            this.sementara = item;
        },

        deletetidak(){
            this.sementara = null;
            this.dialogdelete = false;
        },

        deleteYa(){
            this.todos = this.todos.filter(todo => todo !== this.sementara);
            this.sementara = null;
            this.dialogdelete = false;
        },

        editItem(item){
            this.formTodo = {
                task: item.task,
                priority: item.priority,
                note:  item.note,
            }
            this.sementara = item;
            this.dialogedit = true;
        },

        cancelEdit(){
            this.resetForm();
            this.dialogedit = false;
        },

        updateItem(){
            this.save();
            this.todos = this.todos.filter(todo => todo !== this.sementara);
            this.sementara = null;
            this.dialogedit = false;
        },
        getColor (priority) {
            if (this.todos.priority = "Penting") return 'red'
            else if (this.todos.priority = "Tidak penting") return 'green'
        else return 'blue'
      },
    },
};
</script>
