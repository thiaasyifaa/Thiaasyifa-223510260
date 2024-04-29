<template>
  <div>
    <ActivityTable :activities="activities" @toggle-completion="toggleCompletion" @remove-activity="removeActivity" />
    <h2>Add Your To Do List Here!</h2>
    <form @submit.prevent="submitForm">
      <div>
        <label for="activityName">Activity  : </label>
        <input type="text" id="activityName" v-model="activityName" required>
      </div>
      <div>
        <label for="activityDate">Date  :</label>
        <input type="date" id="activityDate" v-model="activityDate" required>
      </div>
      <button type="submit">Submit Activities</button>
    </form>
  </div>
</template>

<script>
import { ref } from 'vue';
import ActivityTable from './components/ActivityTable.vue';

export default {
  components: {
    ActivityTable
  },
  setup() {
    const activities = ref([]);
    const activityName = ref('');
    const activityDate = ref('');

    const submitForm = () => {
      activities.value.push({
        name: activityName.value,
        date: activityDate.value,
        completed: false
      });
      activityName.value = '';
      activityDate.value = '';
    };

    const toggleCompletion = index => {
      activities.value[index].completed = !activities.value[index].completed;
    };

    const removeActivity = index => {
      activities.value.splice(index, 1);
    };
    
    return { activities, activityName, activityDate, submitForm, toggleCompletion, removeActivity };
  }
};
</script>
