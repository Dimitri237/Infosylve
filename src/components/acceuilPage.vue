<template>
  <div class="container">
    <nav>
      <p>Bienvenue {{ username }}</p>
     <div>
        <button style="color: grey; font-size: 20px; font-weight: bold;" @click="show1"><i class="fa fa-user"></i></button>
        <button style="color: red; font-size: 20px; font-weight: bold;" @click="logout"><i class="fa fa-power-off"></i></button></div>
    </nav>
    <!-- <div v-if="isLoading">Chargement...</div> -->
    <div class="content">
      <ul>
      
        <li v-for="module in modules" :key="module.id">{{ module.libelle }}</li>
      </ul>
    </div>
  </div>
  <div v-if="showModal" @click="hide" class="modal">
    <createAccount />
  </div>
  <div v-if="showModal2" @click="hide" class="modal">
   <div class="options monda-font animate__animated animate__fadeInDown">
    <h4><i class="fa fa-list"></i>UserList</h4>
    <h4 @click="show"><i class="fa fa-users"></i>AddUser</h4>
    <h4><i class="fa fa-users"></i>Parametre</h4>
   </div>
  </div>
</template>

<script>
import axios from "axios";
import createAccount from "./CreateAccount.vue";

export default {
  components: {
    createAccount
  },
  data() {
    return {
      modules: [],
      username: "",
      showModal: false,
      showModal2: true,
      isLoading: false,
    };
  },
  mounted() {
    this.userName = localStorage.getItem("userName");
    this.fetchModules();
  },
  methods: {
    logout() {
      localStorage.removeItem("token");
      localStorage.removeItem("username");
      this.$router.push("/");
    },
    show(){
        this.showModal = true;
    },
    hide(){
        // this.showModal = false;
        this.showModal2 = false;
    },
    show1(){
        this.showModal2 = true;
    },
    async fetchModules() {
      this.isLoading = true; // Optionnel : état de chargement
      
      try {
        const token = localStorage.getItem("jwt"); // Remplacez 'authToken' par la clé que vous utilisez
        const response = await axios.get("http://localhost:8081/auth/modules", {
          headers: {
            Authorization: `Bearer ${token}`, // Ajoutez le token aux en-têtes
          },
        });
        this.modules = response.data;
        console.log(this.modules);
      } catch (error) {
        console.error("Erreur lors de la récupération des modules :", error);
      } finally {
        this.isLoading = false; // Optionnel : réinitialiser l'état de chargement
      }
    },
  },
};
</script>
<style scoped>
.container {
    width: 100%;
    margin: 0 auto;
    background-color: rgba(0, 0, 0, 0.5);
    height: 100vh;
}
nav{
    width: 98%;
    display: flex;
    padding: 5px 1%;
    justify-content: space-between;
    align-items: center;
    background-color: #f1f1f1;
}
h3 {
    font-size: 20px;
    color: #025e1f;
}
ul {
    list-style: none;
    padding: 10px;
    display: flex;
    flex-wrap: wrap; /* Permet de passer à la ligne */
}
li {
    flex: 0 0 22%;
    box-sizing: border-box; 
    text-align: center;
    margin: 100px auto;
    padding: 50px 10px;
    background-color: #f1f1f1;
    color: #025e1f;
    border-radius: 5px;
    font-weight: bold;
    font-size: 25px;
    transition: all 0.3s;
}
li:hover {
    background-color: #afaeae;
    cursor: pointer;
}
button {
    padding: 5px 10px;
    background-color: #f1f1f1;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}
button:hover {
    background-color: #e1e1e1;
}
.modal{
    position: absolute;
    background-color: rgba(0, 0, 0, 0.5);
    width: 100%;
    height: 100vh;
    top: 0;
}
.options{
    width: 20%;
    height: 40vh;
    margin-top: 20vh;
    background-color: #f1f1f1;
    margin-left: 40%;
    border-radius: 10px;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.options h4{
    font-size: 20px;
    color: #025e1f;
    cursor: pointer;
}
</style>
