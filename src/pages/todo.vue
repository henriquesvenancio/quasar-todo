<template>
  <q-page class="bg-grey-3 column">
    <div class="q-pa.sn bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Escreva o Item"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in task"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTasks(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
  data() {
    return {
      newTask: "",
      task: [
        //{
        //  title: "Get bananas",
        //  done: false,
        //},
        //{
        //  title: "Eat bananas",
        //  done: false,
        // },
        //{
        //  title: "Poo bananas",
        //  done: false,
        //},
      ],
    };
  },
  methods: {
    deleteTasks(index) {
      this.$q
        .dialog({
          dark: true,
          title: "Confirmação!",
          message: "Deseja delectar o item?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.task.splice(index, 1);
          this.$q.notify("Item delectado!");
        });
    },
    addTask() {
      this.task.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
