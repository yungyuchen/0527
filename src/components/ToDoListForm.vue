<template>

  <!-- Now we're successfully (成功地) accomplishing (完成) the "Read" (讀取) portion (部分) of a CRUD(Create, Read, Update, Delete) app, but the next most important thing to do is add the ability(能力) to create a new todolist. I'll create an add todolist form.
       Make ToDoListForm. vue and set (設置) it up a field (欄位) to enter (輸入) table, and a button to submit (提交). 
       I'll go ahead(開始一個計劃 行動) and create an todolist data property (屬性) with table on it. -->

  <!-- 創建一個名為文件 ToDoListForm.vue 在 src/components -->

  <div id="todolist-form">
    <!-- <form v-on:submit="todolistSubmit"> -->
    <!-- <form v-on:click="todolistSubmit"> -->
    <form @submit.prevent="todolistSubmit"><!-- Event listeners 事件監聽 當表單提交時觸發 todolistSubmit 事件-->
      <label>ToDoList</label>

      <!-- Now I have to figure out (弄清楚) how to get the data that I'm writing in the input into Vue's component state (組件狀態). 
           To do that I'll use v-model. v-model is some built-in Vue syntactic sugar (語法糖) for updating an input value with an onchange event. -->
      
      <!-- 語法糖讓程式更加簡潔，有更高的可讀性。 -->

      <input v-model="todolist.table" type="text" /><!-- 當他輸入值 觸發 onchange 事件時 則 todolist.table 值會即時更新 updating an input value with an onchange event -->
      <button>Add ToDoList</button>
    </form>
  </div>
</template>

<script>
  export default {
    name: 'todolist-form',
    data() {
      return {
        todolist: {
          table: '',
        },
      }
    },
    methods: { //create first method object on a component.
      todolistSubmit(){ // 事件監聽 方法
        this.$emit('add:todolist', this.todolist) // In our case, I'll create an event called add:todolist, and pass this.todolist.
      // Now if you try to submit the form, you'll see the message logged in the console.
      // We know the form submit method is working properly, so we can pass the data up to App now. I'll do this using $emit.
      // Emit broadcasts a name of an event and data to its parent component (父主件), like so.

      // The add:todolist syntax (句子結構) (as opposed to (相對於) add-todolist or something else (其他的東西)) is recommended (推薦) in the Vue documentation (說明書)
      // Vue文檔中建議使用 add:employee 語法（相對於 add-employee 其他語法）

      // Once you add this (一旦妳添加它之後) , click to add form button and go to Vue DevTools (開發人員工具). You'll see a notification (通知) for a new event, and it will tell you the name, source, and payload (有效負載), which in this case(在這種情況下) is an object we created.
      },
    },
  }
</script>

<style scoped>
/* scoped僅應用於當前組件的元素 */
  form {
    margin-bottom: 2rem;
  }
</style>