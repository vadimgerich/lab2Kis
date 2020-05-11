<template>
  <div id="app">    
    <messageSystem v-bind:messagesList="messagesList"></messageSystem>
    <searchString v-model="searchText"></searchString>
    <queryForm v-model="queryObject"></queryForm>
    <input type="button" @click="showNewBugaltForm()" value=" Додати кнгу" />
    <bugaltTable v-bind:bugaltList="filtredBugalts" @remove="removeBugalt" @update="showUpdateBugaltForm"></bugaltTable>
    <bugaltForm v-model="bugalt" v-bind:visible="formVisible" @input="formInput"></bugaltForm>
  </div>
</template>

<script>
import Vue from "vue";
import bugaltTable from "./components/bugaltTable";
import bugaltForm from "./components/bugaltForm";
import searchString from "./components/searchString";
import queryForm from "./components/queryForm";
import messageSystem from "./components/messageSystem";
import axios from "axios";

export default {
  //назва компоненту, співпадає із назвою файла  
  name: "app",
  //компоненти які використовуються в додатку
  components: {
    bugaltTable,
    bugaltForm,
    searchString,
    queryForm,
    messageSystem
  },
  //дані компонента на початку
  data() {
    return {
      bugalts: [],
      bugalt: {
        pib:"" ,
        posada:"",
        payment: 0,
        countOfChilds: 0,
        stage: 0
      },
      //список повідомлень
      messagesList: [],
      //видимість форми
      formVisible: false,
      selectedIndex: -1,
      //текст пошуку
      searchText: "",
      //об'єкт з параметрами пошуку 
      queryObject: {
        posada:"",
        countOfChilds:0
      }
    };
  },
  // значення які необхідно обчислити.  
  computed: {
    filtredBugalts() {
      let result = this.bugalts;
      if (this.searchText)
        result = result.filter(bugalt =>
          bugalt.title.toLowerCase().includes(this.searchText.toLowerCase())
        );
      console.log(result);
      if (this.queryObject.countOfChilds)
        result = result.filter(bugalt => bugalt.countOfChilds >= this.queryObject.countOfChilds);
      console.log(result);
      if (this.queryObject.posada)
        result = result.filter(bugalt => bugalt.posada <= this.queryObject.posada);
      return result;
    }
  },
  mounted() {
    this.getBugalts().then(bugalts => {
      this.bugalts = bugalts;
    });
  },
  //методи 
  methods: {
    async getBugalts() {
      try {
        //чекаємо відповідь на запит GET  http://localhost:5000/bugalt
        let resp = await axios.get("http://localhost:5000/bugalt");
        //якщо все добре то повертаємо данні із відповіді на запит 
        return resp.data;
      } catch (e) {
        //якщо сталась помилка (в тому числі і сервер повернув 500)
        console.log(e);
        //додати повідомлення 
        this.messagesList.push(e);
      }
    },
    async postBugalt(bugalt) {
      try {
        let resp = await axios.post("http://localhost:5000/bugalt", bugalt);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },
    async deleteBugalt(bugalt) {
      try {
        let resp = await axios.delete(`http://localhost:5000/bugalt/${bugalt._id}`);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },

    async patchBugalt(bugalt) {
      try {
        let resp = await axios.patch(
          `http://localhost:5000/bugalt/${bugalt._id}`,
          bugalt
        );
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },
 
    showNewBugaltForm() {
      this.bugalt = Object.assign(this.bugalt, {
        pib: "",
        posada: "",
        payment: ,
        countOfChilds: ,
        stage: 0,
      });
      this.formAction = this.addNewBugalt;
      this.formVisible = true;
    },

    showUpdateBugaltForm(index) {
      this.selectedIndex = index;
      Object.assign(this.bugalt, this.filtredBugalts[index]);
      this.formAction = this.updateBugalt;
      this.formVisible = true;
    },

    addNewBugalt() {
      //спочатку робимо хук 
      this.postBugalt(this.bugalt).

      then(bugalt => this.bugalts.push(bugalt));
    },

    removeBugalt(index) {
      this.deleteBugalt(this.filtredBugalts[index]).
      then(() => {
        this.filtredBugalts.splice(index, 1);
      });
    },

    updateBugalt() {
      let i = this.selectedIndex;
      this.patchBugalt(this.bugalt).then(bugalt => {
        Object.assign(this.filtredBugalts[i], bugalt);
      });
      this.selectedIndex = -1;
    },

    formInput() {
      this.formAction();
      this.formVisible = false;
    },
    formAction: function() {}
  }
};
</script>

<style scoped>
#app {
  margin: 0;
  padding: 0;
}
</style>
