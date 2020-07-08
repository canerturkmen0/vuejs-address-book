<template>
  <b-container fluid class="d-flex justify-content-center flex-grow-1">
    <b-row class="justify-content-md-center text-center" align-v="center">
      <h1>Welcome to Address Book application developed with Vue.js</h1>
    </b-row>
  </b-container>
</template>

<script>
export default {
  head() {
    return {
      title: "Home | Address Book",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Address Book application developed with Vue.js"
        }
      ]
    };
  }
};
</script>