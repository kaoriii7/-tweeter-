<template>
  <div>
    <table>
      <tr>
        <th>ID</th>
        <th>NAME</th>
        <th>EMAIL</th>
        <th>UPLOAD</th>
        <th>DELETE</th>
      </tr>
      <tr v-for="item in contactLists" :key="item.id">
        <td>{{ item.id }}</td>
        <td><input type="text" v-model="item.name" /></td>
        <td><input type="email" v-model="item.email" /></td>
        <td>
          <button @click="updateContact(item.id, item.name, item.email)">
            更新
          </button>
        </td>
        <td>
          <button @click="deleteContact(item.id)">削除</button>
        </td>
      </tr>
    </table>
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
    async updateContact(id,name,email) {
      const sendData = {
        name: name,
        email: email,
      };
      await this.$axios.put(
        "http://127.0.0.1:8000/api/contact/" + id,
        sendData
      );
      this.getContact();
    },
    async deleteContact(id) {
      await this.$axios.delete("http://127.0.0.1:8000/api/contact/" + id);
      this.getContact();
    },
  },
  created() {
    this.getContact();
  },
}
</script>

<style>

</style>
