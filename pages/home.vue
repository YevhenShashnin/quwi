<template>
  <section>
    <TheHeader />
    <div class="projects-list">
      <ProjectCard
        v-for="(project, index) in projects"
        :project="project"
        :key="project.id"
        @getProjects="getProjects"
      />
    </div>
  </section>
</template>

<script>
import axios from "axios";

export default {
  name: "HomePage",
  data() {
    return {
      projects: [],
    };
  },
  methods: {
    getProjects: function () {
      axios
        .get("https://api.quwi.com/v2/projects-manage", {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("jwt"),
          },
        })
        .then((resposnse) => {
          this.projects = resposnse.data.projects;
        })
        .catch((error) => {
          console.log(error);
          this.$router.push("/");
        });
    },
  },
  mounted() {
    this.getProjects();
  },
};
</script>

<style>
body {
  margin: 0;
  background: #f4f4f4;
}
.projects-list {
  padding-top: 66px;
  margin: 0 auto 50px;
  width: 760px;
}
</style>
