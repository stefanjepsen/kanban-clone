<template>
  <q-page>
    <!-- page content -->

    <div class="q-pa-md row">
      <div class="col-8 offset-2">
        <AddTodo />
        <FilterTodos />

        <div class="row q-gutter-xs">
          
          <div v-for="todo in allTodos" :key="todo.id" class="col-3 offset-1">
            <transition
              appear
              enter-active-class="animated fadeIn slow"
              leave-active-class="animated fadeOut slow"
            >
              <q-card dark bordered class="bg-grey-9 my-card">
                <q-card-section>
                  <div class="">{{ todo.title }}</div>
                  <div class="text-subtitle2">by John Doe</div>
                </q-card-section>

                <q-separator dark inset />

                <q-card-section>
                  <q-icon
                    @click="deleteTodo(todo.id)"
                    class="it"
                    size="sm"
                    name="print"
                  />
                </q-card-section>
              </q-card>
            </transition>
          </div>
        </div>
      </div>
    </div>
    <div class="q-pa-md row q-gutter-md"></div>
  </q-page>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import AddTodo from "src/components/AddTodo";
import FilterTodos from "src/components/FilterTodos";

export default {
  name: "Board",
  components: {
    AddTodo,
    FilterTodos
  },
  methods: {
    ...mapActions(["fetchTodos", "deleteTodo"]),
  },
  computed: mapGetters(["allTodos"]),
  created() {
    this.fetchTodos();
  },
};
</script>

<style>
.it {
  position: absolute;
  bottom: 5px;
  right: 5px;

  cursor: pointer;
}
</style>