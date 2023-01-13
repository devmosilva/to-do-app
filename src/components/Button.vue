<template>
    <button :class="class" >{{textBtn}}</button>
</template>

<script>
    export default {
        props: ['text', 'class'],
        computed: {
            textBtn(){
                return this.text ?  this.text : ''
            }
        }
    }
</script>


<style scoped>


  .btn-add {
        background-color: #008000;
        border: none;
        color: #FFFFFF;
        padding: 8px;
        border-radius: 6px;
  }

    .btn-remove {
       background-color: #FA8072;
        border: none;
        color: #FFFFFF;
        padding: 8px;
        border-radius: 6px;
  }

  .btn-add:hover {
        background-color: #006400;
        color:white;
    }

    .btn-remove:hover {
        background-color: #DC143C;
        color:white;
    }


</style>
