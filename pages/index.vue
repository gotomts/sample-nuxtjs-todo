<template>
  <section class="container">
    <h1>Todoリスト</h1>
    <div class="add-area">
      <input type="text" name="add-name" id="add-name" v-model="content" placeholder="タスクを入力してください">
      <button id="add-button" class="button button--green" @click="insert">追加</button>
    </div>
    <div class="filter">
      <button class="button button-gray" v-bind:class="{
        'is-active': (!findFlg)
      }" @click="flgReset">全て</button>
      <button class="button button-gray" :class="{
        'is-active': findFlg && (findState == '作業前')
      }" @click="find('作業前')">作業前</button>
      <button class="button button-gray" :class="{
        'is-active': findFlg && (findState == '作業中')
      }" @click="find('作業中')">作業中</button>
      <button class="button button-gray" :class="{
        'is-active': findFlg && (findState == '完了')
      }" @click="find('完了')">完了</button>
    </div>
    <table class="lists">
      <thead>
        <tr>
          <th>タスク</th>
          <th>登録日時</th>
          <th>状態</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in displayTodos" :key="index">
          <td>{{ item.content }}</td>
          <td>{{ item.created }}</td>
          <td>
            <button class="button"
              :class="{
                'button--yet': item.state === '作業前',
                'button--progress': item.state === '作業中',
                'button--done': item.state === '完了'
              }"
              @click="changeState(item)"
              >{{ item.state }}</button>
          </td>
          <td>
            <button class="button button--red" @click="remove(item)">削除</button>
          </td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
import { mapState } from "vuex";

export default {
  data() {
    return {
      content: "",
      findState: "",
      findFlg: false
    }
  },
  computed: {
    ...mapState(["todos"]),
    displayTodos() {
      if (this.findFlg) {
        const arr = [];
        const data = this.todos;
        data.forEach(element => {
          if (element.state == this.findState) {
            arr.push(element);
          }
        });
        return arr;
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    insert() {
      if (this.content != "") {
        this.$store.commit("insert", {
          content: this.content
        });
        this.content = "";
      }
    },
    remove(todo) {
      this.$store.commit("remove", todo);
    },
    changeState(todo) {
      this.$store.commit("changeState", todo);
    },
    find(findState) {
      this.findState = findState;
      this.findFlg = true;
    },
    flgReset() {
      this.findFlg = false;
    }
  }
};
</script>

<style>
.container {
  width: 640px;
  margin: 30px auto;
}
h1 {
  margin-bottom: 20px;
  text-align: center;
}
input {
  width: 400px;
  padding: 10px;
  border-radius: 5px;
}
th, td {
  padding: 10px 20px;
  border-collapse: collapse;
}
.button {
  width: 80px;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid gray;
  background: #fff;
  font-weight: bold;
}
.button:hover {
  cursor: pointer;
}
.button.is-active {
  border-color: #000;
  background: #000;
  color: #fff;
}
.button--green {
  border-color: green;
  color: green;
}
.button--red {
  border-color: red;
  color: red;
}
.button--progress {
  background: blue;
  color: white;
}
.button--done {
  background: black;
  color: white;
}
.add-area {
  margin-bottom: 20px;
}
.filter {
  margin-bottom: 20px;
}
.lists {
  margin-bottom: 20px;
}
.lists th {
  border-bottom: 2px solid #000;
}
.lists th:first-child {
  width: 200px;
}
.lists td {
  border-bottom: 1px solid #000;
}
</style>
