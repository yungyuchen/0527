<template>
  <div id="app" class="small-container">
    <h1>ToDoList</h1>

    <div id="todolist-form">
      <!-- <form v-on:submit="todolistSubmit"> -->
      <!-- <form v-on:click="todolistSubmit"> -->
      <form>
        <label>ToDoList Form</label>
        <input v-model="todolist.table" type="text" />
        <button @click.prevent="addTodolist(todolist.table,sort1)">Add ToDoList</button>
        <button @click.prevent="sort1 = 3">無期限</button>
        <button @click.prevent="sort1 = 2">次要</button>
        <button @click.prevent="sort1 = 1">優先</button>
        <button @click.prevent="sort1 = 0">全部</button>
      </form>
    </div>
    <!-- 下拉存值 -->
    <div id="todolist-table">
      <!-- <h1>Vue Select</h1>
      <select v-model="sort">
        <option value disabled selected>--請選擇--</option>
        <option value="0">--全部--</option>
        <option value="1">--優先--</option>
        <option value="2">--次要--</option>
        <option value="3">--無期限--</option>
      </select>
      <div>sort = {{ sort }}</div> -->

      <div>
        <h2>Todolist Select</h2>
        <button @click="sort = 3">無期限</button>
        <button @click="sort = 2">次要</button>
        <button @click="sort = 1">優先</button>
        <button @click="sort = 0">全部</button>
      </div>

      <p v-if="todolists.length < 1" class="empty-table">No ToDoList</p>
      <div v-else>
        <h5>Todolist Table</h5>
        <div v-for="todolist in todolists" :key="todolist.id">
          <div class="tab-content" v-if="sort == 0">
            <div v-if="todolist.sort !== sort">
              <div class="tab-text" v-if="editing === todolist.id">
                <input type="text" v-model="todolist.table" />
              </div>
              <div class="tab-text" v-else>{{ todolist.table }}</div>
              <div class="tab-button" v-if="editing === todolist.id">
                <button @click="editTodolists(todolist.id,todolist.table)">Save</button>
                <button class="muted-button" @click="editing = null">Cancel</button>
              </div>
              <div class="tab-button" v-else>
                <button @click="editMode(todolist.id,todolist.sort)">Edit</button>
                <button @click="deleteTodolists(todolist.id)">Delete</button>
              </div>
            </div>
          </div>
          <div class="tab-content" v-if="todolist.sort === sort">
            <div class="tab-text" v-if="editing === todolist.id">
              <input type="text" v-model="todolist.table" />
            </div>
            <div class="tab-text" v-else>{{ todolist.table }}</div>
            <div class="tab-button" v-if="editing === todolist.id">
              <button @click="editTodolists(todolist.id,todolist.table)">Save</button>
              <button class="muted-button" @click="editing = null">Cancel</button>
            </div>
            <div class="tab-button" v-else>
              <button @click="editMode(todolist.id,todolist.sort)">Edit</button>
              <button @click="deleteTodolists(todolist.id)">Delete</button>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      sort: 0,
      todolist: {
        table: "" //使用者新輸入的事項
      },
      todolists: [
        //存取每一筆事項的空陣列
        {
          id: 1,
          sort: 3, // 1 優先 2 次要 3 無限期
          table: "熟悉Git指令"
        },
        {
          id: 2,
          sort: 2, // 1 優先 2 次要 3 無限期
          table: "安裝Vue建置環境"
        },
        {
          id: 3,
          sort: 1, // 1 優先 2 次要 3 無限期
          table: "整理技術筆記"
        }
      ],
      editing: null
    };
  },
  created() {
    // vue 生命週期
    this.todolists = JSON.parse(localStorage.getItem("todolists"));
  },

  methods: {
    editMode(id) {
      this.editing = id;
    },

    addTodolist(todolist, sort) {
      const lastId =
        this.todolists.length > 0
          ? this.todolists[this.todolists.length - 1].id
          : 0; // 判斷原有的 todolists 是否有資料, 有 取得最後一個ID , 無 則等於0
      const id = lastId + 1; // 定義一個ID變數取 新字串的ID
      this.todolists.push({ id: id, sort: sort, table: todolist });
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
      for (let index = 0; index < this.todolists.length; index++) {
        if (this.todolists[index].id === id) {
          this.todolists[index].table == updatedTodolist;
        }
      }
      this.editing = null;
      localStorage.setItem("todolists", JSON.stringify(this.todolists));
    },
   
  }
};
</script>

<style>
/* 添加了一些全局樣式 */
form {
  margin-bottom: 2rem;
}
select {
    line-height: 22px;
}

h2 {
  margin: 100px 0 10px 0;
}

h5 {
  border-bottom: 1px solid #dedede;
  margin: 60px 0 0 0;
}

.tab-content {
  display: inline-block;
  width: 100%;
  border-bottom: 1px solid #dedede;
  padding: 0.5rem;
  vertical-align: middle;
}

.tab-text {
  display: inline-block;
  width: 70%;
  vertical-align: middle;
  margin: -6px;
}

.tab-button {
  display: inline-block;
  width: 30%;
  min-width: 200px;
  vertical-align: middle;
}

button {
  margin: 0 0 0 0.5rem;
  float: right;
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>