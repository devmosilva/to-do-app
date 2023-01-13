<template>
        <div class="add">
              <label for='taskx'></label>
              <input placeholder="Escreva aqui sua nota"  class="input-task" name='taskx' type="text" v-model="task">
              <Button text="Adicionar" @click="addToDo()" class="btn-add" />
        </div>

        <div  v-for='item in toDos' v-bind:key='item.id' class="content" >
          <label >
              <input class="checkbox-todo" :value="item.status"  type="checkbox" @change='doneTodo($event ,item)' > 
          </label>
          
             <label >
              <input id='input-todo' disabled="!editTodo" :class="{'input-todo': true, 'todo-done' : item.status == true ? true : '' }" type="text" :value="item.value" > 
          </label>
              <Button text="Deletar" @click="removeTodo(item)" class="btn-remove" />
          
        </div>
</template>

<script  lang="ts">
import Button from "./Button.vue"

  export default {
    components: {Button},
    data () {
      return {
        task: '',
        toDos: [],
      }
    },
    methods: {
      async addToDo(){
        if(this.task == ''){
            this.$toast.error(`Campo não pode ser vazio !`);
            return false
        }
        const newTodo = {
          id: Math.floor(Math.random() * 100),
          value: this.task,
          status: false 
        }

        this.task = '';
        this.toDos.push(newTodo)
        
        localStorage.setItem('todos', JSON.stringify(this.toDos));

        this.$toast.success(`Adicionado com sucesso !`);

      },
      removeTodo(item){
        this.$toast.error(`Removido com sucesso !`);
        this.toDos = this.toDos.filter(todo => todo !== item)
      },
      doneTodo(ev,item){
        item.status = !item.status;
        localStorage.setItem('todos', JSON.stringify(this.toDos));
      }
    },
    mounted() {
      if (localStorage.todos) {
            this.toDos = JSON.parse(localStorage.getItem('todos') || [])
        }
    },
    watch: {
      deep: true,
      immediate: true,
      toDos(newToDo){
        localStorage.setItem('todos', JSON.stringify(newToDo));
      },
    }
  }
</script>

<style scoped>

.todo-done {
    text-decoration: line-through;
}

.content {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid #000;
    padding: 5px;
    border-radius: 8px;
    margin-bottom: 10px;
  }

.add {
  display: flex;
  align-items: center;
  flex-direction: center;
  margin-bottom: 15px;
}

.input-task {
  margin-right: 10px;
  border-color: blue;
  border-radius: 6px;
  padding: 8px;

}

.input-todo {
  border: none;
  margin-right: 10px;
  border-color: blue;
  border-radius: 6px;
  padding: 8px;

}

.checkbox-todo {
  width: 1.3em;
  height: 1.3em;
  background-color: white;
  border-radius: 50%;
  vertical-align: middle;
  border: 1px solid #696969;
  -webkit-appearance: none;
  outline: none;
  cursor: pointer;
}

.checkbox-todo:checked {
  background-color: #A9A9A9	;
}

.input-todo:focus {
  outline: 0;
}

</style>
