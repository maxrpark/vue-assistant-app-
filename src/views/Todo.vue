<template>
  <div class="section-center section">
    <h2 className="title">Vue assistant</h2>
    <Alert :alert="alert" :showAlert="showAlert" />
    <section class="main-container">
      <h2>Task List</h2>
      <Form ref="form" @handleSubmit="handleSubmit" />
      <div class="todo-list">
        <ul class="todo-container">
          <li
            v-for="item in todo"
            :key="item.id"
            :class="{ isComplete: item.isComplete }"
            class="item"
          >
            <TodoItem
              :item="item"
              @editItem="editItem"
              @completeItem="completeItem"
              @deleteItem="deleteItem"
            />
          </li>
        </ul>
      </div>

      <button v-if="todo.length" class="remove-items" @click="removeAll">
        remove all
      </button>
    </section>
  </div>
</template>

<script>
import TodoItem from '../components/TodoItem.vue';
import Alert from '../components/Alert.vue';
import Form from '../components/Form.vue';
export default {
  components: {
    TodoItem,
    Alert,
    Form,
  },
  mounted() {
    this.setItems();
  },
  data() {
    return {
      todo: [],
      task: '',
      isEditing: false,
      itemId: null,
      showAlert: false,
      timeOut: null,
      alert: {
        type: '',
        messege: '',
        show: false,
      },
    };
  },
  methods: {
    // get all Items
    setItems() {
      this.todo = localStorage.getItem('vuetodoList');
      if (this.todo) {
        return (this.todo = JSON.parse(localStorage.getItem('vuetodoList')));
      } else {
        this.todo = [];
        localStorage.setItem('vuetodoList', JSON.stringify(this.todo));
      }
    },
    // remove all
    handleSubmit(task) {
      this.task = task;
      if (this.task.length && this.task.trim() !== '') {
        if (!this.isEditing) {
          const newTask = {
            id: new Date().getTime().toString(),
            value: this.task,
            isComplete: false,
          };
          this.todo.push(newTask);
          this.alertFunction('success', 'Task added');
        } else {
          this.todo.map((item) => {
            if (item.id === this.itemId) {
              item.value = this.task;
            }
          });
          this.itemId = null;
          this.isEditing = false;
          this.alertFunction('success', 'Task Edited');
        }
        this.$refs.form.task = '';

        localStorage.setItem('vuetodoList', JSON.stringify(this.todo));
      } else {
        this.alertFunction('danger', 'Task can not be empty!');
      }
    },
    // remove edit
    editItem(id) {
      this.isEditing = true;
      this.todo.map((item) => {
        if (item.id === id) {
          this.task = item.value;
        }
      });
      this.alertFunction('warning', 'Editing...');
      this.itemId = id;
    },
    // remove complete
    completeItem(id) {
      this.isEditing = false;
      this.$refs.form.task = '';
      this.todo.map((item) => {
        if (item.id === id) {
          if (!item.isComplete) {
            this.alertFunction('success', 'Task Completed');
          }
          item.isComplete = !item.isComplete;
        }
      });
      localStorage.setItem('vuetodoList', JSON.stringify(this.todo));
    },
    // remove delete
    deleteItem(id) {
      this.isEditing = false;
      this.$refs.form.task = '';
      this.todo = this.todo.filter((item) => {
        return item.id !== id;
      });
      this.alertFunction('danger', 'Task Deleted');
      localStorage.setItem('vuetodoList', JSON.stringify(this.todo));
    },
    // remove all
    removeAll() {
      this.todo = [];
      this.$refs.form.task = '';
      this.editItem = false;
      this.alertFunction('danger', 'All tasks removed');
      localStorage.setItem('vuetodoList', JSON.stringify(this.todo));
    },
    alertFunction(type, messege) {
      clearTimeout(this.timeOut);
      this.showAlert = true;
      this.alert = {
        type: type,
        messege: messege,
      };
      if (!this.isEditing) {
        this.timeOut = setTimeout(() => {
          this.showAlert = false;
        }, 1500);
      }
    },
  },
};
</script>

<style></style>
