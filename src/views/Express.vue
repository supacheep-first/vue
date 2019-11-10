<template>
  <div class="express-page text-left">
    <p>fn: get:/GET</p>
    <pre>{{ resGET }}</pre>
    <p>fn: getByAge:/GET</p>
    <pre>{{ resGetByAge }}</pre>

    <button @click="createStudent">add more student</button>
    <br />
    <button @click="addAgeStudent">rendom add age some student 5 year.</button>
    <br />
    <button @click="deletes">delete some one.</button>
  </div>
</template>

<script>
import Axios from "axios";
export default {
  name: "express-page",
  data() {
    return {
      resGET: String,
      resGetByAge: String
    };
  },
  methods: {
    get() {
      Axios("http://localhost:8081/").then(res => {
        this.resGET = res.data;
      });
    },
    getByAge(age) {
      Axios("http://localhost:8081/" + age).then(res => {
        this.resGetByAge = res.data;
      });
    },
    createStudent() {
      Axios.post("http://localhost:8081/create/student", {
        name: "Khal Drogo",
        age: "18"
      }).then(res => {
        console.log(res);
        this.get();
      });
    },
    addAgeStudent() {
      Axios.put("http://localhost:8081/add/age/" + 5).then(res => {
        console.log(res);
        this.get();
      });
    },
    deletes() {
      Axios.delete("http://localhost:8081/").then(() => {
        this.get();
      });
    }
  },
  mounted() {
    this.get();
    this.getByAge(15);
  }
};
</script>
<style scoped>
.text-left {
  text-align: ltft;
}
</style>
