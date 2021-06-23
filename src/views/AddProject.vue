<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input type="text" v-model="title" required />
    <label>Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Add Project</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      details: "",
    };
  },
  methods: {
    handleSubmit() {
      let project = {
        title: this.title,
        details: this.details,
        completed: false,
      };
      fetch("http://localhost:3000/projects", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(project),
      })
        .then(this.redirectHome())
        .catch((err) => console.log(err.message));
    },
    redirectHome() {
      setTimeout(() => {
        this.$router.push("/");
      }, 100);
    },
  },
};
</script>

<style lang="scss" scoped>
form {
  text-align: left;
  background: #fff;
  padding: 20px;
  border-radius: 10px;
  label {
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: calc(0.5em + 1vw);
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0;
  }
  input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
  }
  textarea {
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
  }
  button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: #fff;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
}
</style>