<template>
  <div>
    <h3> Hello There! Organize Your To Do List Here!</h3>
    <h2>Here Are Your To Do List!</h2>
    <table>
      <thead>
        <tr>
          <th>Activity</th>
          <th>Date</th>
          <th>Completed</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(activity, index) in activities" :key="index">
          <td v-if="activity.editing">
            <input type="text" v-model="activity.name">
          </td>
          <td v-else>{{ activity.name }}</td>
          <td v-if="activity.editing">
            <input type="text" v-model="activity.date">
          </td>
          <td v-else>{{ activity.date }}</td>
          <td>
            <input type="checkbox" v-model="activity.completed" @change="toggleCompletion(index)">
          </td>
          <td>
            <button @click="activity.editing = !activity.editing">{{ activity.editing ? 'Save' : 'Edit' }}</button>
            <button @click="removeActivity(index)">Cancel</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    activities: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    toggleCompletion(index) {
      this.$emit('toggle-completion', index);
    },
    removeActivity(index) {
      this.$emit('remove-activity', index);
    }
  }
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

input[type="checkbox"] {
  transform: scale(1.5);
}

</style>
