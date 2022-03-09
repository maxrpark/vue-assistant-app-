<template>
  <div class="section-center section">
    <h2 className="title">Vue assistant</h2>
    <div class="alert-container">
      <div v-if="showAlert" :class="alert.type" class="alert-box">
        <p>{{ alert.messege }}</p>
      </div>
    </div>
    <section class="main-container">
      <h2>Task List</h2>
      <form @submit.prevent="handleSubmit" class="form-container">
        <input
          v-model="task"
          type="text"
          class="input-control"
          placeholder="Type something..."
          maxlength="25"
        />
      </form>

      <div class="todo-list">
        <ul class="todo-container">
          <li
            v-for="item in todo"
            :key="item.id"
            :class="{ isComplete: item.isComplete }"
            class="item"
          >
            <p class="item-value">{{ item.value }}</p>
            <div class="btns-container">
              <button @click="editItem(item.id)" class="btn edit">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M18.363 8.464l1.433 1.431-12.67 12.669-7.125 1.436 1.439-7.127 12.665-12.668 1.431 1.431-12.255 12.224-.726 3.584 3.584-.723 12.224-12.257zm-.056-8.464l-2.815 2.817 5.691 5.692 2.817-2.821-5.693-5.688zm-12.318 18.718l11.313-11.316-.705-.707-11.313 11.314.705.709z"
                  />
                </svg>
              </button>
              <button @click="completeItem(item.id)" class="btn complete">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M20.285 2l-11.285 11.567-5.286-5.011-3.714 3.716 9 8.728 15-15.285z"
                  />
                </svg>
              </button>
              <button @click="deleteItem(item.id)" class="btn delete">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M24 20.188l-8.315-8.209 8.2-8.282-3.697-3.697-8.212 8.318-8.31-8.203-3.666 3.666 8.321 8.24-8.206 8.313 3.666 3.666 8.237-8.318 8.285 8.203z"
                  />
                </svg>
              </button>
            </div>
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
export default {
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
    handleSubmit() {
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
        this.task = '';
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
      this.task = '';
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
      this.task = '';
      this.todo = this.todo.filter((item) => {
        return item.id !== id;
      });
      this.alertFunction('danger', 'Task Deleted');
      localStorage.setItem('vuetodoList', JSON.stringify(this.todo));
    },
    // remove all
    removeAll() {
      this.todo = [];
      this.task = '';
      this.editItem = false;
      this.alertFunction('danger', 'All items removed');
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
