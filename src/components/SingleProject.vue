<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="toggleDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-icons">edit</span>
        </router-link>
        <span class="material-icons" @click="deleteProject">delete</span>
        <span class="material-icons tick" @click="markProject">done</span>
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "SingleProject",
  props: {
    project: Object,
  },
  data() {
    return {
      showDetails: false,
      uri: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails;
    },
    deleteProject() {
      fetch(this.uri, { method: "DELETE" })
        .then(this.$emit("delete", this.project.id))
        .catch((err) => console.log(err.message));
    },
    markProject() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(this.$emit("toggleDone", this.project.id))
        .catch((err) => console.log(err.message));
    },
  },
};
</script>

<style lang="scss" scoped>
.project {
  margin: 2em auto;
  background: #fff;
  padding: 0.5em 3em;
  border-radius: 0.5em;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  border-left: 0.5em solid #e90074;
  text-align: left;

  h3 {
    cursor: pointer;
  }
  .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;

    .material-icons {
      font-size: 24px;
      margin-left: 10px;
      color: #bbb;
      cursor: pointer;
    }
    .material-icons:hover {
      color: #444;
    }
    .material-icons.tick:hover {
      color: #00ce89;
    }
  }
  &.complete {
    border-left: 0.5em solid #00ce89;

    .material-icons.tick {
      color: #00ce89;
    }
    .material-icons.tick:hover {
      color: #e90074;
    }
  }
}
</style>