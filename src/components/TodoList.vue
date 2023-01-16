<template>
        <div class="add">
              <label for='message'></label>
              <input placeholder="Escreva aqui sua nota"  class="input-task" name='message' type="text" v-model="message">
              <Button text="Adicionar" @click="addToDo()" class="btn-add" />
        </div>

        <h4 v-show="toDos.length >= 1"> A Fazer: </h4>

        <div v-show="!item.status" v-for='item in toDos' v-bind:key='item.id' class="content" >
          <label >
              <input class="checkbox-todo" :value="item.status"  :checked="item.status" type="checkbox" @change='doneTodo($event ,item)' > 
          </label>
          <label >
              <input id='input-todo' disabled="!editTodo" :class="{'input-todo': true, 'todo-done' : item.status == true ? true : '' }" type="text" :value="item.message" > 
          </label>
              <Button text="Deletar" @click="removeTodo(item)" class="btn-remove" />
        </div>

        <h4 v-show="toDos.length >= 1"> Completas: </h4>

        <div  v-show="item.status" v-for='item in toDos' v-bind:key='item.id' class="content" >
            <label >
                <input class="checkbox-todo" :value="item.status"  :checked="item.status" type="checkbox" @change='doneTodo($event ,item)' > 
            </label>
            
              <label >
                <input id='input-todo' disabled="true" :class="{'input-todo': true, 'todo-done' : item.status == true ? true : '' }" type="text" :value="item.message" > 
            </label>
                <Button text="Deletar" @click="removeTodo(item)" class="btn-remove" />
        </div>
</template>

<script  lang="ts">
import Button from "./Button.vue"
import axios from 'axios'
import FethApi from '@/services/api';

  export default {
    components: {Button},
    data () {
      return {
        message: '',
        toDos: [],
      }
    },
    created(){
     this.getTodos();
   },
    methods: {
      async getTodos(){
        //   await axios.get('http://127.0.0.1:8000/api/todo/index').then((response) => {
        //     console.log(response)
        //     this.toDos = response;
        //  });
          fetch( 'http://127.0.0.1:8000/api/todo/index', {
              method: 'GET',
              header: {
                'Accept': 'application/json',
                "Access-Control-Allow-Origin": "*"
              }
          } )
          .then(response=>response.json())
          .then(data=>{ this.toDos = data; });
         
      },
      async addToDo(){
        
        if(this.message == ''){
            this.$toast.error(`Campo não pode ser vazio !`);
            return false
        }

        const newTodo = {
          message: this.message,
          status: false
        }

        try {
          const response = await axios.post('http://127.0.0.1:8000/api/todo/create', newTodo);
          this.toDos.push({
            id: response.data.id, 
            message: response.data.message,
            status: response.data.status
           });

          this.message = '';
          this.$toast.success(`Adicionado com sucesso !`);
          
        } catch(erro) {
            this.$toast.error(`Erro ao executar função !`);
        }

        
        // localStorage.setItem('todos', JSON.stringify(this.toDos));


      },
      async removeTodo(item){
        try {
          await axios.delete(`http://127.0.0.1:8000/api/todo/delete/${item.id}`);

          this.toDos = this.toDos.filter(todo => todo !== item);
          this.$toast.success(`Removido com sucesso !`);


        } catch (error) {
            this.$toast.error(`Erro ao executar função !`);
        }

    
      },
      async doneTodo(ev,item){

        try {
          await axios.put(`http://127.0.0.1:8000/api/todo/updateStatus/${item.id}`);

        } catch(err){

            this.$toast.error(`Erro ao executar função !`);
          
        }
        item.status = !item.status;
        // localStorage.setItem('todos', JSON.stringify(this.toDos));
      }
    },
    mounted() {
      if (localStorage.todos) {
            this.toDos = JSON.parse(localStorage.getItem('todos') || [])
        }
    },
    watch: {
      toDos: {
        handler(newToDo) {
          // localStorage.setItem('todos', JSON.stringify(newToDo));
        },
        deep: true
      }
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

  .todo-done-green {
    border-color: green;
  }
.add {
  display: flex;
  align-items: center;
  flex-direction: center;
  margin-bottom: 15px;
}

.input-task {
  margin-right: 10px;
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
