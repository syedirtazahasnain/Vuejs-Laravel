<template>
  <div class="container">
    <div class="row justify-content:center">
      <h1 class="text-center text-dark mt-2 mb-5">Vuejs with vite</h1>
    </div>
    <div class="row">
      <div class="col-md-4">
        <form
          @submit.prevent="save"
          class="max-w-md mx-auto bg-white shadow-md rounded-lg px-8 py-5 dark:bg-gray-800"
        >
          <div class="mb-6">
            <label
              for="text"
              class="block text-sm font-medium text-gray-700 dark:text-gray-200"
              >Student Name</label
            >
            <input
              type="text"
              placeholder="name"
              v-model="student.name"
              class="bg-gray-100 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-indigo-500 focus:border-indigo-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-indigo-500 dark:focus:border-indigo-500"
              required
            />
          </div>
          <div class="mb-6">
            <label
              for="number"
              class="block text-sm font-medium text-gray-700 dark:text-gray-200"
              >Phone</label
            >
            <input
              type="number"
              placeholder="number"
              v-model="student.phone"
              class="bg-gray-100 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-indigo-500 focus:border-indigo-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-indigo-500 dark:focus:border-indigo-500"
              required
            />
          </div>
          <div class="mb-6">
            <label
              for="text"
              class="block text-sm font-medium text-gray-700 dark:text-gray-200"
              >Address</label
            >
            <input
              type="text"
              placeholder="address"
              v-model="student.address"
              class="bg-gray-100 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-indigo-500 focus:border-indigo-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-indigo-500 dark:focus:border-indigo-500"
              required
            />
          </div>
          <button
            type="submit"
            class="text-white bg-indigo-500 hover:bg-indigo-700 focus:ring-4 focus:outline-none focus:ring-indigo-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-indigo-600 dark:hover:bg-indigo-700 dark:focus:ring-indigo-800"
          >
            Submit
          </button>
        </form>
      </div>

      <div class="col-md-8">
        <table class="!table-fixed border shadow-md rounded-lg overflow-hidden">
          <thead>
            <tr class="bg-gray-100 text-gray-700">
              <th class="px-4 py-2 text-left">Sr No</th>
              <th class="px-4 py-2 text-left">Name</th>
              <th class="px-4 py-2 text-left">Address</th>
              <th class="px-4 py-2 text-left">Phone</th>
              <th class="px-4 py-2 text-left">Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="student in result"
              v-bind:key="student.id"
              class="hover:bg-gray-200"
            >
              <td class="!px-4 py-2 border-b border-gray-200">{{ student.id }}</td>
              <td class="!px-4 py-2 border-b border-gray-200">{{ student.name }}</td>
              <td class="!px-4 py-2 border-b border-gray-200">{{ student.address }}</td>
              <td class="!px-4 py-2 border-b border-gray-200">{{ student.phone }}</td>
              <td class="!px-4 py-2 border-b border-gray-200">
                <button
                  type="button"
                  class="bg-blue-500 text-white hover:bg-blue-700 py-1 px-2 rounded focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500"
                  @click="edit(student)">
                  Edit
                </button>
                <button
                  type="button"
                  class="bg-red-500 text-white hover:bg-red-700 py-1 px-2 rounded focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
                  @click="remove(student)">
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "Student",
  data() {
    return {
      result: {},
      student: {
        id: "",
        name: "",
        address: "",
        phone: "",
      },
    };
  },
  created() {
    this.StudentLoad();
  },
  mounted() {
    console.log("mounted() called.....");
  },
  methods: {
    StudentLoad() {
      var page = "http://127.0.0.1:8000/api/student";
      axios.get(page).then(({ data }) => {
        console.log("fetch_api_data", data);
        this.result = data;
      });
    },
    save() {
      if (this.student.id == "") {
        this.saveData();
      } else {
        this.updateData();
      }
    },
    saveData() {
      axios.post("http://127.0.0.1:8000/api/student", this.student).then(({ data }) => {
        alert("saved");
        this.StudentLoad();
        this.student.name = "";
        (this.student.address = ""), (this.student.phone = "");
        this.id = "";
      });
    },
    edit(student) {
      this.student = student;
    },
    updateData() {
      var editrecords = "http://127.0.0.1:8000/api/student/" + this.student.id;
      axios.put(editrecords, this.student).then(({ data }) => {
        this.student.name = "";
        (this.student.address = ""), (this.student.phone = "");
        this.id = "";
        alert("Updated!!!");
        this.StudentLoad();
      });
    },

    remove(student) {
      var url = `http://127.0.0.1:8000/api/student/${student.id}`;
      // var url = 'http://127.0.0.1:8000/api/student/'+ student.id;
      axios.delete(url);
      alert("Deleted");
      this.StudentLoad();
    },
  },
};
</script>
