<template>
  <section class="container">
    <h1>Todoリスト</h1>
    <div class="add-area">
      <input type="text" name="add-name" id="add-name" v-model="content" placeholder="タスクを入力してください">
      <button id="add-button" class="button button--green" @click="insert">追加</button>
    </div>
    <div class="filter">
      <button class="button button-gray is-active">全て</button>
      <button class="button button-gray">作業前</button>
      <button class="button button-gray">作業中</button>
      <button class="button button-gray">完了</button>
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
        <tr v-for="(item, index) in todos" :key="index">
          <td>{{ item.content }}</td>
          <td>{{ item.created }}</td>
          <td>
            <button class="button button--yet">{{ item.state }}</button>
          </td>
          <td>
            <button class="button button--red">削除</button>
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
      content: ""
    }
  },
  computed: {
    ...mapState(["todos"])
  },
  methods: {
    insert() {
      if (this.content != "") {
        this.$store.commit("insert", {
          content: this.content
        });
        this.content = "";
      }
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
