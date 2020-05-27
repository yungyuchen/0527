<template>
  <div id="app" class="small-container">
    <h1>ToDoList</h1>

    <div id="todolist-form">
      <!-- <form v-on:submit="todolistSubmit"> -->
      <!-- <form v-on:click="todolistSubmit"> -->
      <form>
        <label>ToDoList</label>
        <input v-model="todolist.table" type="text" />
        <button @click.prevent="addTodolist(todolist.table)">Add ToDoList</button>
      </form>
    </div>
    <!-- 下拉存值 -->
    <div id="todolist-table">
      <h1>Vue Select</h1>

      <select class="form-control mr-1">
        <option value disabled selected>--請選擇--</option>
        <option value="0" @change="filterTodolists(0)">--全部--</option>
        <option value="1" @change="filterTodolists(1)">--優先--</option>
        <option value="2" @change="filterTodolists(2)">--次要--</option>
        <option value="3" @change="filterTodolists(3)">--無期限--</option>
      </select>

      <button @click="filterTodolists(0)">全部</button>
      <button @click="filterTodolists(1)">優先</button>
      <button @click="filterTodolists(2)">次要</button>
      <button @click="filterTodolists(3)">無期限</button>

      <p v-if="todolists.length < 1" class="empty-table">No ToDoList</p>
      <table v-else>
        <thead>
          <tr>
            <th>ToDoList</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="todolist in todolists" :key="todolist.id">
            <td v-if="editing === todolist.id">
              <input type="text" v-model="todolist.table" />
            </td>
            <td v-else>{{ todolist.table }}</td>
            <td v-if="editing === todolist.id">
              <button @click="editTodolists(todolist.id,todolist.table)">Save</button>
              <button class="muted-button" @click="editing = null">Cancel</button>
            </td>
            <td v-else>
              <button @click="editMode(todolist.id)">Edit</button>
              <button @click="deleteTodolists(todolist.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      newtodolist: [
        {
          id: "",
          sort: "", // 1 優先 2 次要 3 無限期
          table: ""
        }
      ],
      todolist: {
        table: "" //使用者新輸入的事項
      },
      todolists: [
        //存取每一筆事項的空陣列
        {
          id: 1,
          sort: 2, // 1 優先 2 次要 3 無限期
          table: "熟悉Git指令"
        },
        {
          id: 2,
          sort: 1,
          table: "安裝Vue建置環境"
        },
        {
          id: 3,
          sort: 3,
          table: "整理技術筆記"
        }
      ],
      editing: null
    };
  },
  created() {
    // vue 生命週期
    this.todolists = JSON.parse(localStorage.getItem("todolists"));
    // this.todolists = this.newtodolist;
    // this.todolists=  this.ne
  },

  methods: {
    editMode(id) {
      this.editing = id;
    },

    addTodolist(todolist) {
      console.log("addTodolist = ");
      const lastId =
        this.todolists.length > 0
          ? this.todolists[this.todolists.length - 1].id
          : 0; // 判斷原有的 todolists 是否有資料, 有 取得最後一個ID , 無 則等於0
      const id = lastId + 1; // 定義一個ID變數取 新字串的ID
      this.todolists.push({ id: id, table: todolist });
      localStorage.setItem("todolists", JSON.stringify(this.todolists));
      this.todolist.table = "";
    },

    deleteTodolists(id) {
      this.todolists = this.todolists.filter(function(todolist) {
        return todolist.id !== id;
      });
      localStorage.setItem("todolists", JSON.stringify(this.todolists));
    },

    editTodolists(id, updatedTodolist) {
      // console.log("id = " + id); // 1 2 3
      // const abcid = this.todolists[id].table;
      for (let index = 0; index < this.todolists.length; index++) {// 0 1 2
        if (this.todolists[index].id === id) { // 1
          console.log("this.todolists[index].table = " + this.todolists[index].table); // 熟悉Git指令
          console.log("updatedTodolist = " + updatedTodolist); // 熟悉Git指令111
           this.todolists[index].table == updatedTodolist;
        }
      }
      this.editing = null;
      localStorage.setItem("todolists", JSON.stringify(this.todolists));
      
      // this.todolists.push({ id: id, table: updatedTodolist });

      // console.log(" this.todolists[id].table = " + abcid); // 1 2 3
      // if (this.todolists.id === id) {
        // this.todolists[id].table == updatedTodolist;
      // }
      //   if (updatedTodolist === "") return; // 防呆 對話視窗
      //   this.todolists = (id, updatedTodolist);
      //   this.editing = null; // 恢復預設的編輯狀態

      // 傳進ToDoListTable 的 id updatedTodolist 參數 map (映射) through the todolists array, and update the correct (正確的) todolist.
      //  map builds a new array

      // this.todolists = this.todolists.map( todolist => todolist.id === id ? updatedTodolist : todolist.table ) // ES6寫法
      // 傳入處理中 array 物件 的 value index

      // this.todolists.table = this.todolists.map(function(todolist) {
      // const newtodolist = this.todolists.map(function(todolist) {
      //   // 跑迴圈執行 為每個 array element  執行一次提供的 function 功能
      //   // 類似 forEach() method executes (執行) a provided(供給) function (功能) once for each (每個各一次) array (陣列) element(元素).
      //   // 類似 forEach() 為每一個陣列元素都提供了一次執行的功能 都run過一次
      //   console.log("todolist.id = " + todolist.id); // 1 2 3
      //   console.log("todolist.table = " + todolist.table); // 1 2 3
      //   console.log("id = " + id); // 1
      //   console.log("updatedTodolist = " + updatedTodolist); // object array 物件
      //   console.log("todolist = " + todolist.table); // object  array 物件
      //   // if (todolist.id === id) {
          
      //   // }
      //   return todolist.id == id ? updatedTodolist : todolist.table; // 判斷 todolist.id === id 則等於 updatedTodolist 否則為 原本的 todolist
      // });
      // this.todolists = newtodolist;
    },

    filterTodolists(sort) {
      console.log("sort +" + sort);
      if (sort === 0) {
        this.todolists = this.todolists.filter(function(todolist) {
          return todolist.sort !== sort;
        });
      } else {
        this.todolists = this.todolists.filter(function(todolist) {
          return todolist.sort == sort;
        });
      }
    }
    // printValue(val) {
    //   alert("Functions parameter: " + val);
    //   alert("Binding value before tick: " + this.value);
    //   this.$nextTick(() => {
    //     alert("Binding value after tick: " + this.value);
    //   });
    // }
  }
};
</script>

<style>
form {
  margin-bottom: 2rem;
}

button {
  margin: 0 0.5rem 0 0;
  float: right;
}

/* 添加了一些全局樣式 */
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>