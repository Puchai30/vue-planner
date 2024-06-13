<template>
  <div class="single_project" :class="{ complete: single_project.complete }">
    <div class="flexing">
      <div>
        <h3 @click="showDetail = !showDetail">
          {{ single_project.title }}
        </h3>
      </div>
      <div>
        <span class="material-symbols-outlined" @click="deleteProject"> delete </span>

        <router-link :to="{name:'editproject', params:{id:single_project.id} }">
          <span class="material-symbols-outlined"> edit </span>
        </router-link>
        
        <span class="material-symbols-outlined" @click="completeProject"> done </span>
      </div>
    </div>
    <p v-if="showDetail">{{ single_project.detail }}</p>
    {{ single_project.complete }}
  </div>
</template>

<script>
export default {
  props: ["single_project"],
  data() {
    return {
      showDetail: false,
      api: "http://localhost:3000/projects/",
    };
  },
  methods: {
    deleteProject() {
      let deletRoute = this.api + this.single_project.id;
      fetch(deletRoute, { method: "DELETE" })
        .then(() => {
          this.$emit("delete", this.single_project.id);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    completeProject() {
      let updateCompleteRoute = this.api + this.single_project.id;
      fetch(updateCompleteRoute, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          complete: !this.single_project.complete,
        }),
      })
        .then(() => {
          this.$emit("complete", this.single_project.id);
        })
        .catch ((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style>
.single_project {
  padding: 20px;
  background-color: rgb(236, 241, 241);
  border-left: 6px solid crimson;
  border-radius: 8px;
  margin: 10px;
}
h3 {
  color: indigo;
  cursor: pointer;
}
.flexing {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
span {
  margin-left: 10px;
}
span:hover {
  color: #777;
  cursor: pointer;
}
.complete {
  border-left-color: green;
}
</style>
