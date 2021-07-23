<template>
  <div id="app">
    <h1>Japan Post API Example</h1>
    <h2>日本郵便番号検索</h2>

    <form @submit.prevent="onSubmit">
      <label for="q">郵便番号</label>
      <input id="q" placeholder="000-0000" v-model="q">
      
      <input :disabled="!$data.q || $data.loading" type="submit">
    </form>

    <p v-if="$data.error && !$data.loading">{{ $data.error }}</p>

    <p v-if="$data.address && !$data.loading">
      {{ $data.address.province }}
      {{ $data.address.city }}
      {{ $data.address.town }}
    </p>
  </div>
</template>

<script>
// const parseAddress = data => ({
//   zipCode: data.zipCode,
//   province: data.province,
//   city: data.city,
//   town: data.town,
//   _province: data._province,
//   _city: data._city,
//   _town: data._city
// });

export default {
  name: "App",
  data() {
    return {
      loading: false,
      q: "",
      error: "",
      address: null
    };
  },
  methods: {
    async onSubmit() {
      const { q } = this.$data;
      const API = `https://jp-post.herokuapp.com/api/find?q=${q}`;
      this.$data.loading = true;
      this.$data.error = "";
      this.$data.address = null;
      fetch(API)
        .then(async res => {
          const data = await res.json();
          if (data.error) {
            this.$data.error = data.error;
          } else {
            this.$data.address = data;
          }
        })
        .catch(err => {
          alert(err);
        });
      this.$data.loading = false;
    }
  }
};
</script>

