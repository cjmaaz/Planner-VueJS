<template>
  <div class="home">
    <FilterNav @filterProject="status = $event" :status="status" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="deleteProject"
          @toggleDone="markProject"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "@/components/SingleProject.vue";
import FilterNav from "@/components/FilterNav.vue";
export default {
  name: "Home",
  data() {
    return {
      projects: [],
      status: "all",
    };
  },
  components: {
    SingleProject,
    FilterNav,
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    deleteProject(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    markProject(id) {
      let p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    },
  },
  computed: {
    filteredProjects() {
      if (this.status === "completed") {
        return this.projects.filter((project) => project.complete);
      } else if (this.status === "ongoing") {
        return this.projects.filter((project) => !project.complete);
      }
      return this.projects;
    },
  },
};
</script>
