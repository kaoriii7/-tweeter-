<template>
  <div class="app" id="app">
    <div class="new">
      <h1>連絡先アプリ</h1>
        <h2>新規作成</h2>
        <div>
          <label for="name">お名前：　</label>
          <input type="text" name="name" id="name" v-model="newName" />
        </div>
        <div>
          <label for="email">メールアドレス：　</label>
          <input type="email" name="email" id="email" v-model="newEmail" />
        </div>
        <button @click="insertContact">新規作成</button>
    </div>
    <div>
      <h2>連絡先リスト</h2>
      <div v-for="item in contactLists" :key="item.id">
        <Message :item="item" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newName: "",
      newEmail: "",
      contactLists: [],
    };
  },
  methods: {
    async getContact() {
      const resData = await this.$axios.get(
        "http://127.0.0.1:8000/api/contact/"
      );
      this.contactLists = resData.data.data;
    },
    async insertContact() {
      const sendData = {
        name: this.newName,
        email: this.newEmail,
      };
      await this.$axios.post("http://127.0.0.1:8000/api/contact/", sendData);
    },
  },
  created() {
    this.getContact();
  },
};
</script>

<style>
.app {
  display: flex;
}
</style>
