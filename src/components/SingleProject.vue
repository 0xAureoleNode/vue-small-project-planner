<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h4 @click="showDetails = !showDetails">{{ project.title }}</h4>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-symbols-outlined">edit</span>
        </router-link>
        <span @click="deleteProject" class="material-symbols-outlined"
          >delete</span
        >
        <span @click="toggleComplete" class="material-symbols-outlined tick"
          >done</span
        >
      </div>
    </div>
    <div class="details" v-if="showDetails">{{ project.details }}</div>
  </div>
</template>

<script>
export default {
  props: ['project'],
  data() {
    return {
      showDetails: false,
      url: 'http://localhost:3000/projects/' + this.project.id,
    };
  },
  methods: {
    deleteProject() {
      fetch(this.url, { method: 'DELETE' })
        .then(() => this.$emit('delete', this.project.id))
        .catch((err) => console.error(err));
    },
    toggleComplete() {
      fetch(this.url, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => this.$emit('complete', this.project.id))
        .catch((err) => console.error(err));
    },
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
.project.complete {
  border-left: 4px solid #00ce89;
}
.project.complete .tick {
  color: #00ce89;
}
h4 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-symbols-outlined {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-symbols-outlined:hover {
  color: #777;
}
</style>
