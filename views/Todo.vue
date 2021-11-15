<template>
  <div class="home">
    <v-list class="pt-0" flat>
      <v-text-field
        v-model="newTask"
        class="pa-4 pt-8"
        @click:append="addTask()"
        @keydown.enter="addTask()"
        label="add Task"
        append-icon="mdi-plus-box-multiple"
        clearable
      ></v-text-field>
      <div v-if="tasks.length > 0">
        <div v-for="task in tasks" :key="task.id">
          <v-list-item
            @click="doneTask(task.id)"
            :class="{ 'blue lighten-5 ': task.done }"
          >
            <template v-slot:default>
              <v-list-item-action>
                <v-checkbox
                  :input-value="task.done"
                  color="primary"
                ></v-checkbox>
              </v-list-item-action>

              <v-list-item-content>
                <v-list-item-title>{{ task.title }}</v-list-item-title>
              </v-list-item-content>

              <v-list-item-action>
                <v-menu>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn color="white" v-bind="attrs" v-on="on" icon>
                      <v-icon color="primary lighten-1"
                        >mdi-dots-vertical</v-icon
                      >
                    </v-btn>
                  </template>
                  <v-list dense rounded>
                    <v-list-item @click.stop="deleteTask(task.id)">
                      <v-list-item-icon>
                        <v-icon>mdi-delete</v-icon>
                      </v-list-item-icon>
                      <v-list-item-content>
                        <v-list-item-title>delete Task</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                    <v-list-item @click.stop="editTask(task.id)">
                      <v-list-item-icon>
                        <v-icon>mdi-comment-edit</v-icon>
                      </v-list-item-icon>
                      <v-list-item-content>
                        <v-list-item-title>edit Task</v-list-item-title>
                      </v-list-item-content>
                    </v-list-item>
                  </v-list>
                </v-menu>
                <v-card
                  v-if="task.editable"
                  class="mx-auto edit-task-field"
                  max-width="400"
                >
                  <v-list-item>
                    <!-- <p>{{ index }}</p> -->
                    <v-list-item-content>
                      <v-list-item-title class="pb-5"
                        >editTask</v-list-item-title
                      >
                      <v-form @submit.prevent @keydown="editTask(task.id)">
                        <v-container>
                          <v-row>
                            <v-col cols="12">
                              <v-text-field
                                @keyup.enter="saveTask(task.id)"
                                v-model="edTask"
                                label="Text"
                                clearable
                              ></v-text-field>
                            </v-col>
                          </v-row>
                        </v-container>
                      </v-form>
                      <v-btn
                        @click.stop="saveTask(task.id)"
                        rounded
                        color="primary"
                        dark
                      >
                        Save
                      </v-btn>
                    </v-list-item-content>
                  </v-list-item>
                </v-card>
              </v-list-item-action>
            </template>
          </v-list-item>
          <v-divider></v-divider>
        </div>
      </div>
      <div v-else class="d-flex flex-column justify-center align-center">
        <v-icon color="#90caf9" x-large>mdi-check-bold</v-icon>
        <p class="pt-3 text--disabled font-weight-bold" color="#90caf9">
          No Tasks
        </p>
      </div>
    </v-list>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [],
      edTask: "",
    };
  },
  methods: {
    addTask() {
      let newTask = {
        id: Date.now(),
        title: this.newTask,
        done: false,
        editable: false,
      };
      if (this.newTask.trim()) {
        this.tasks.push(newTask);
      }
      this.newTask = "";
    },
    doneTask(id) {
      let task = this.tasks.filter((task) => task.id === id)[0];
      if (!task.editable) {
        task.done = !task.done;
      }
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    editTask(id) {
      let task = this.tasks.filter((task) => task.id === id)[0];
      this.edTask = task.title;
      task.editable = !task.editable;
      return this.edTask;
    },
    saveTask(id) {
      let task = this.tasks.filter((task) => task.id === id)[0];
      task.title = this.edTask;
      task.editable = !task.editable;
    },
  },
};
</script>

<style scoped>
.edit-task-field {
  position: fixed;
  bottom: 50vh;
  left: 50%;
  transform: translate(-50%, 50%);
  width: 95%;
  padding: 5% 0;
  z-index: 9999;
}
</style>
