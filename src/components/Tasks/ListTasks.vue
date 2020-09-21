<template>
    <div>
        <h2>{{title}}</h2>

        <form class="form mt-3 mb-3">
            <input type="text" class="form-control" v-model="filter" placeholder="Busque pelo nome">
        </form>

        <form class="form form-inline mt-3 mb-3" @submit.prevent="onSubmit">
            <input required class="form-control" ref="inputName" type="text" placeholder="Informe o nome" v-model="task.name">
            <button type="submit" class="btn btn-success">{{txtBtn}}</button>
        </form>

        <div class="table-responsive">
            <table class="table table-sm table-hover table-striped">
                <caption>Lista de Tarefas</caption>
                <thead class="thead-dark">
                    <tr>
                        <th>ID</th>
                        <th>NOME</th>
                        <th width="200px">AÇÕES</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(task, index) in filteredItems" :key="index">
                        <td style="vertical-align: middle">{{task.id}}</td>
                        <td style="vertical-align: middle">{{task.name}}</td>
                        <td>
                            <a class="btn btn-warning" href="#" @click.prevent="edit(task.id)">Editar</a>
                            <a class="btn btn-danger" href="#"  @click.prevent="deleteTask(task.id)">Deletar</a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            title: 'Lista de tarefas',
            txtBtn: 'Cadastrar',
            tasks: [],
            task: {id:'',name:''},
            updating: false,
            updatedIndex: '',
            filter: ''
        }
    },
    methods: {
        onSubmit() {
            if(this.updating){
                this.update()
                return
            }

            this.save()
        },
        save() {
            this.verDuplicate()
            this.task.id = this.tasks.length +1
            this.tasks.push(this.task)
            this.clearForm()
        },
        edit(id) {
            this.updatedIndex = this.findIndexItem(id)
            
            this.task = this.tasks[this.updatedIndex]

            this.updating = true
            this.txtBtn = 'Salvar Edição'
            this.focusElement()
        },
        update() {
            this.verDuplicate()
            this.tasks[this.updatedIndex] = this.task
            this.updating = false
            this.txtBtn = 'Cadastrar'
            this.clearForm()
        },
        clearForm() {
            this.task = {id:'', name:''}
            this.focusElement()
        },
        deleteTask(id) {
            this.tasks.splice(this.findIndexItem(id), 1)
            this.focusElement()
        },
        focusElement() {
            this.$refs.inputName.focus();
        },
        findIndexItem(id) {
            for (let index = 0; index < this.tasks.length; index++) {
                if(this.tasks[index].id === id)
                    return index
                
            }
        },
        verDuplicate() {
            this.tasks.filter(task => {
                if(task.name == this.task.name){
                    alert("Esse item já existe! Mas vamos deixar você cadastrar")
                }
            })
        }
    },
    computed: {
        filteredItems() {
            if(this.filter === '')
                return this.tasks      

            let vm = this

            return this.tasks.filter(task => {
                return task.name.indexOf(vm.filter) > -1
            })
            
            // return this.tasks.filter(task => {
            //     return task.name.toLowerCase().indexOf(vm.filter.toLowerCase()) > -1
            // })

            // return this.tasks.filter(task => {
            //     return task['name'].includes(vm.filter)
            // })
        }
    },
}
</script>

<style scoped>
    
</style>