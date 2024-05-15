<template>
  <div class="container">
    <div class="d-flex align-items-center justify-content-between">
      <div class="d-flex align-items-center">
        <img
          alt="Vue logo"
          style="width: 50px; aspect-ratio: 1/1"
          class="me-2"
          src="./assets/logo.png"
        />
        <h1 class="mb-0">Todo List</h1>
      </div>
      <button @click="handlerRemoveItem" class="btn btn-outline-danger">
        Remove
      </button>
    </div>
    <!-- main -->
    <section id="main">
      <ul class="list-group pt-2">
        <template v-if="items.length == 0">
          <li class="list-group-item">Belum ada item yang ditambahkan.</li>
        </template>
        <li v-for="(item, index) in items" :key="index" class="list-group-item">
          <input
            class="form-check-input me-1"
            type="checkbox"
            :id="`todolist${index}`"
            @click="handleClickItem(index)"
            v-model="items[index].checked"
          />
          <label
            class="form-check-label stretched-link"
            :for="`todolist${index}`"
            >{{ item.name }}</label
          >
        </li>
      </ul>
      <hr class="mt-4" />
      <template v-if="editItem">
        <div class="d-flex align-items-center justify-content-between">
          <h1>Edit Item</h1>
          <button @click="handlerCancelEdititem" class="btn btn-outline-danger">
            Batalkan
          </button>
        </div>

        <form action="#" @submit.prevent="handleEditItem">
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control"
              placeholder="Item Name"
              v-model="tempItem"
            />
            <button class="btn btn-outline-primary" type="submit">
              Edit Item
            </button>
          </div>
        </form>
      </template>
      <template v-else>
        <h1>Add Item</h1>
        <form action="#" @submit.prevent="handleSubmitItem">
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control"
              placeholder="Item Name"
              v-model="tempItem"
            />
            <button class="btn btn-outline-primary" type="submit">
              Add Item
            </button>
          </div>
        </form>
      </template>
    </section>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      items: [],
      tempItem: "",
      editItem: false,
      selectedItem: 0,
    };
  },
  methods: {
    handleSubmitItem() {
      this.items.push({ name: this.tempItem, checked: false });
      this.tempItem = "";
      localStorage.setItem("items", JSON.stringify(this.items));
    },
    handlerRemoveItem() {
      this.items.forEach((element, index) => {
        if (element.checked) {
          this.items.splice(index, 1);
        }
      });
      this.editItem = false;
      this.tempItem = "";
      localStorage.setItem("items", JSON.stringify(this.items));
    },
    handleEditItem() {
      this.items[this.selectedItem] = { name: this.tempItem, checked: false };
      this.tempItem = "";
      localStorage.setItem("items", JSON.stringify(this.items));
      this.selectedItem = 0;
      this.editItem = false;
    },
    handleClickItem(index) {
      this.items.forEach((element, idx) => {
        console.log(index, idx);
        console.log(index != idx);
        if (index != idx) {
          //   this.items[idx].checked = false;
          element.checked = false;
        } else {
          if (!element.checked) {
            this.selectedItem = index;
            this.tempItem = this.items[index].name;
            this.editItem = true;
          } else {
            this.selectedItem = 0;
            this.tempItem = "";
            this.editItem = false;
          }
        }
      });
    },
    handlerCancelEdititem() {
      this.items.forEach((element, idx) => {
        element.checked = false;
      });
      this.editItem = false;
    },
  },
  mounted() {
    this.items = [
      ...this.items,
      ...(localStorage.getItem("items")
        ? JSON.parse(localStorage.getItem("items"))
        : []),
    ];
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
