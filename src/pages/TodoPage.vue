<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addNewTask"
        class="col"
        bg-color="white"
        square
        filled
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addNewTask" />
        </template>
      </q-input>
    </div>
    <q-list bordered separator class="bg-white">
      <q-item
        v-for="(task, index) in tasks"
        :key="index"
        clickable
        v-ripple
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            class="no-pointer-events"
            v-model="task.done"
            bg-color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label :class="{ 'text-strike': task.done }">{{
            task.title
          }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            dense
            round
            color="primary"
            icon="delete"
            @click.stop="deleteItem(index)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h4 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue';
import { useQuasar } from 'quasar';

const $q = useQuasar();

interface Task {
  title: string;
  done: boolean;
}

const newTask = ref<string>('');
const tasks = ref<Task[]>([]);

const addNewTask = () => {
  tasks.value.push({
    title: newTask.value,
    done: false,
  });
  newTask.value = '';
};

const deleteItem = (index: number) => {
  $q.dialog({
    title: 'Confirm',
    message: 'Are you sure?',
  }).onOk(() => {
    tasks.value.splice(index, 1);
    $q.notify('Task has been deleted!');
  });
};
</script>

<style lang="scss" scoped>

.no-tasks{
  opacity: 0.5;
}
</style>
