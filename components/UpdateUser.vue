<template>
  <div class="container">
    <div>
      <b-row>
        <b-col cols="12" md="6">
          <h3>Issue description</h3>
          <ol>
            <li>Create a new user</li>
            <li>Create a new Todo for that user</li>
            <li>The update returns an object with the Todo</li>
            <li>But the Todo is not inserted into the store</li>
          </ol>
        </b-col>
        <b-col cols="12" md="6">
          <h4>Code used to update</h4>
          <pre>
methods: {
  createUser(evt) {
    evt.preventDefault();
    this.User.insert({ data: { name: this.newUsername } });
  },
  createTodo(evt) {
    this.User.update({
      where: this.selectedUser,
      data: {
        todos: [
          {
            title: this.newTodoTitle,
            user_id: this.selectedUser
          }
        ]
      }
    })
  }
}
            </pre>
        </b-col>
      </b-row>

      <div class="section">
        <!-- Users creation  -->
        <h4>Create users</h4>
        <b-form @submit.prevent="createUser">
          <b-input-group prepend="Username" class="mt-3">
            <b-form-input v-model="newUsername" autocomplete="off" />
            <b-input-group-append>
              <b-button type="submit" variant="info">Create</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form>
        <b-table striped hover :items="users" />
        <template v-if="users.length == 0">No users yet.</template>
      </div>

      <div class="section">
        <!-- TodoS Creation  -->
        <h4>Create Todos</h4>
        <b-form @submit.prevent="createTodo">
          <b-input-group class="mt-3">
            <b-form-select v-model="selectedUser" :options="userSelectOptions" />
            <b-form-input v-model="newTodoTitle" autocomplete="off" />
            <b-input-group-append>
              <b-button type="submit" variant="info">Create</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form>
        <b-table striped hover :items="todos" />
        <template v-if="todos.length == 0">No todos yet.</template>
      </div>
    </div>

    <div class="section">
      <h4>Insert result</h4>
      <b-card>{{ insertResult }}</b-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newUsername: "",
      newTodoTitle: "",
      selectedUser: "",
      insertResult: null
    };
  },
  computed: {
    User() {
      return this.$store.$db().model("users");
    },
    Todo() {
      return this.$store.$db().model("todos");
    },
    users() {
      return this.User.query()
        .with("todos")
        .get();
    },
    todos() {
      return this.Todo.query()
        .with("assignee")
        .get();
    },
    userSelectOptions() {
      const options = [];
      for (const user of this.users) {
        options.push({ value: user.id, text: user.name });
      }
      return options;
    }
  },
  methods: {
    createUser(evt) {
      evt.preventDefault();
      this.User.insert({ data: { name: this.newUsername } });
    },
    createTodo(evt) {
      this.User.update({
        data: {
          id: this.selectedUser,
          todos: [
            {
              title: this.newTodoTitle,
              user_id: this.selectedUser
            }
          ]
        },
        insert: ["todos"]
      }).then(r => (this.insertResult = r));
    }
  }
};
</script>

<style>
.section {
  margin-top: 2em;
  margin-bottom: 4em;
}
</style>
