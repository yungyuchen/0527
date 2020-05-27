<template>

  <!-- 創建一個名為文件 ToDoListTable.vue 在 src/components -->

  <div id="todolist-table">
    <p v-if="todolists.length < 1" class="empty-table"> <!-- 判斷如果沒有ToDoList 則顯示 No ToDoList -->
      No ToDoList
    </p>
    <table v-else> <!-- 判斷如果有ToDoList 則顯示表單 -->
      <thead>
        <tr>
          <th>ToDoList</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="todolist in todolists" :key="todolist.id"> <!-- I'll do this with the v-for attribute(屬性). retrieve (檢索) todolists in ToDoListseTable, I'll display (排列) one table row (表欄) per (每一個) todolist. -->
          <!-- Vue has a requirement (規定要求) for uniquely (唯一地) identifying (識別) any element (元素) in an array, so I'll use :key on the table row and set (放置) it to a unique (唯一地) value. -->
          <td v-if="editing === todolist.id"> <!-- 判斷為編輯狀態時 -->
            <input type="text" v-model="todolist.table" />  <!-- 則顯示 input 可修改 todolist.table 的值 -->
          </td>
          <td v-else>{{ todolist.table }}</td> <!-- 若不是為編輯狀態時 直接顯示 todolist.table 值在表格上 -->
          <td v-if="editing === todolist.id"> <!-- 判斷為編輯狀態時 顯示 Save Cancel 按鈕 -->
            <button @click="editTodolists(todolist)">Save</button> <!-- 新增 Save 按鈕 並傳 todolist 值給 editTodolists 事件 -->
            <button class="muted-button" @click="editing = null">Cancel</button> <!-- 新增 Cancel 按鈕 並把 editing = null 編輯狀態取消 -->
          </td>
          <td v-else> <!-- 若不是為編輯狀態時  顯示 Edit Delete 按鈕 -->
            <button @click="editMode(todolist.id)">Edit</button> <!-- 新增 Edit 按鈕 並傳欲編輯的 todolist.id 值給 editMode 事件 -->
            <button @click="$emit('delete:todolist', todolist.id)">Delete</button> <!-- 新增 Delete 按鈕和名為 delete:todolist click 事件 並$emit傳 todolist.id 值給父主件 delete:todolist 事件 -->
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'todolist-table',
    props: { // Now on the ToDoListTable side, we want to retrieve that data(資訊檢索), so we tell the component(組件) that it will receive(收到) props(道具), in this case (在這種情況下為) an Array (陣列).
      todolists: Array,
    },
    data(){
      return{
        editing:null, // 命名一個data來記錄編輯狀態 預設為null 無編輯中
      }
    },
    methods:{
      editMode(id){ // 傳入所點擊要編輯的 todolist.id
        this.editing =id // 紀錄此 id 正在編輯中
      },

      editTodolists(todolist){
        if(todolist.table ==='') return // 防呆 對話視窗
        this.$emit('edit:todolist' ,todolist.id, todolist)
        this.editing = null // 恢復預設的編輯狀態
      }
    }
  }
</script>

<style scoped>
 button {
    margin: 0 0.5rem 0 0;
    float: right;
  }
</style>