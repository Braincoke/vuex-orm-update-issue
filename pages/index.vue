<template>
  <div class="container">
    <div>
      <h1 class="title">
        Vuex-ORM Update Issue
      </h1>
      <h2 class="subtitle">
        Updating a relationship yields unexpected results
      </h2>
      <b-tabs content-class="mt-3">
        <b-tab @click="deleteAll" title="Todo.insert" active>
          <insert-todo />
        </b-tab>
        <b-tab @click="deleteAll" title="User.update">
          <update-user />
        </b-tab>
      </b-tabs>
    </div>
  </div>
</template>

<script>
import InsertTodo from '@/components/InsertTodo'
import UpdateUser from '@/components/UpdateUser'

export default {
  components: {
    InsertTodo,
    UpdateUser
  },
  computed: {
    User () {
      return this.$store.$db().model('users')
    },
    Todo () {
      return this.$store.$db().model('todos')
    },
  },
  methods: {
    deleteAll() {
      this.User.deleteAll()
      this.Todo.deleteAll()
    }
  }
}
</script>

<style>
.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 20px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
