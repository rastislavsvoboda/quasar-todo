<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            color="primary"
            icon="delete"
            dense
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"></q-icon>
      <div class="div text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "Todo",
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Get bananas",
        //   done: false,
        // },
        // {
        //   title: "Eat bananas",
        //   done: true,
        // },
        // {
        //   title: "Poo bananas",
        //   done: false,
        // },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Really delete?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          const taskTitle = this.tasks[index].title;
          this.tasks.splice(index, 1);
          this.$q.notify(`Task '${taskTitle}' deleted`);
        });
    },
    addTask() {
      if (this.newTask != "") {
        this.tasks.push({
          title: this.newTask,
          done: false,
        });
        this.newTask = "";
      }
    },
  },
  mounted() {
    console.log("TODO :: mounted");
  },
  unmounted() {
    console.log("TODO :: unmounted");
  },
  activated() {
    console.log("TODO :: activated");
  },
  deactivated() {
    console.log("TODO :: deactivated");
  },
});
</script>

<style lang="scss" scoped>
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>
