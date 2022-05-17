<template>
  <section>
    <div class="b-project-content" @click="showModal">
      <div class="b-logo">
        <div class="b-avatar">
          <img width="51" height="51" :src="project.logo_url" />
        </div>
      </div>
      <div class="b-name">
        <div class="e-name">{{ project.name }}</div>
      </div>
      <div class="b-active">
        <div class="e-active" v-if="project.is_active == 1">Active</div>
      </div>
      <div class="b-time">time this week {{ project.spent_sec_all_time }}</div>
    </div>
    <CustomModal v-show="isModalVisible" @close="closeModal">
      <template v-slot:header> Project </template>

      <template v-slot:body>
        <CustomInput
          :value="projectName.value"
          :type="projectName.type"
          :error="projectName.error"
          :placeholder="projectName.placeholder"
          @change="projectNameHandler"
        />
        <button @click="saveProjectName" :disabled="isSent">save</button>
      </template>
    </CustomModal>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "ProjectCard",
  props: {
    project: {
      type: Object,
    },
    getProjects: {
      type: Function,
    },
  },
  data() {
    return {
      isModalVisible: false,
      isSent: false,
      projectName: {
        value: this.project.name,
        type: "text",
        error: false,
        placeholder: "name",
      },
    };
  },
  methods: {
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    projectNameHandler(value) {
      this.projectName.value = value;
    },
    saveProjectName() {
      this.isSent = true;
      axios
        .post(
          "https://api.quwi.com/v2/projects-manage/update",
          {
            name: this.projectName.value,
          },
          {
            headers: {
              Authorization: "Bearer " + localStorage.getItem("jwt"),
            },
            params: {
              id: this.project.id,
            },
          }
        )
        .then((resposnse) => {
          this.closeModal();
          this.$emit("getProjects");
        })
        .catch((error) => {
          console.log(error);
          this.isSent = false;
        });
    },
  },
};
</script>

<style lang="css">
.b-project-content {
  display: flex;
  align-items: center;
  background-color: #fff;
  border-radius: 8px;
  border: 1px solid #dedede;
  padding: 25px;
  cursor: pointer;
  font-size: 0.9em;
  margin-top: 10px;
}
.b-name {
  flex: 2;
  overflow: hidden;
}
.b-name .e-name {
  display: block;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  max-width: 100%;
  font-weight: 500;
  font-size: 14px;
  font-family: GoogleSans;
}
.b-logo {
  border: none;
  padding: 0;
  flex: 1;
  margin-right: 25px;
  max-width: 65px;
  cursor: pointer;
}
.e-active {
  color: green;
  font-size: 1.5rem;
}
.b-time {
  margin-left: 20px;
}
</style>
