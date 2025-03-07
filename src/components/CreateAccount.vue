<template>
  <div class="container monda-font animate__animated animate__fadeInDown">
    <nav>
      <img src="" alt="" />
    </nav>
    <div>
      <h2 class="monda-font">Ajouter un Utilisateur</h2>
    </div>
    <form @submit.prevent="createAccount">
      <div class="input-field">
        <div><label for="name">Nom</label></div>
        <input class="input" type="text" id="name" v-model="name" required />
      </div>
      <div class="input-field">
        <div><label for="name">Prenom</label></div>
        <input class="input" type="text" id="surname" v-model="surname" required />
      </div>
      <div class="input-field">
        <div><label for="name">Sexe</label></div>
        <select id="gender" v-model="gender" required>
          <option value="Masculin">Masculin</option>
          <option value="Feminin">Feminin</option>
        </select>
      </div>
      <div class="input-field">
        <div><label for="email">Email</label></div>
        <input class="input" type="" id="email" v-model="email" required />
      </div>
      <div class="input-field">
        <div><label for="birthDate">Date de naissance</label></div>
        <input class="input" type="date" id="birthDate" v-model="birthDate" required />
      </div>

      <div class="input-field">
        <div><label for="mobilePhone">Telephone</label></div>
        <input class="input" type="" id="mobilePhone" v-model="mobilePhone" required />
      </div>
      <div class="input-field">
        <div><label for="roles">Type d'utilisateur</label></div>
        <select id="roles" v-model="roles" required>
          <option value="SUPER_ADMIN">SUPER_ADMIN</option>
        </select>
      </div>
      <button class="btn" type="submit">
        <span>Inscription</span>
      </button>
    </form>
    <div v-if="successMessage" class="success-message">{{ successMessage }}</div>
    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      name: "",
      surname: "",
      gender: "",
      email: "",
      birthDate: "",
      mobilePhone: "",
      roles: [],
      successMessage: "",
      errorMessage: "",
    };
  },
  methods: {
    async createAccount() {
      try {
        const token = localStorage.getItem("jwt");
        if (this.birthDate) {
          const timestamp = new Date(this.birthDate).getTime();
          console.log("Timestamp:", timestamp);

          // Si roles est une chaîne de caractères, convertissez-la en tableau
          const rolesArray = Array.isArray(this.roles) ? this.roles : [this.roles];

           axios.post(
            "http://localhost:8081/utilisateurs/save",
            {
              name: this.name,
              surname: this.surname,
              gender: this.gender,
              email: this.email,
              birthDate: timestamp,
              mobilePhone: this.mobilePhone,
              roles: rolesArray, // Utilisez le tableau ici
            },
            {
              headers: {
                Authorization: `Bearer ${token}`,
              },
            }
          );
          console.log("Inscription réussie !");
          this.$router.push('/acceuilPage');
        } else {
          this.errorMessage = "Veuillez sélectionner une date.";
          console.error(this.errorMessage);
        }
      } catch (error) {
        this.errorMessage = "Erreur lors de la création du compte. Veuillez réessayer.";
        console.error("Erreur lors de la création du compte:", error);
      }
    },
  },
};
</script>

<style scoped>
h2 {
  font-size: 25px;
  font-weight: bold;
  color: #025e1f;
}

.stext {
  color: rgba(0, 0, 0, 0.2);
  font-size: 13px;
  margin-top: -20px;
}

.container {
  background-color: white !important;
  border-radius: 10px;
  text-align: left;
  width: 40% !important;
  height: 90vh !important;
  margin: auto;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2) !important;
  padding: 0 15px;
  margin-top: 35px !important;
}

form {
  width: 100%;
  margin-top: 20px;
}

.monda-font {
  font-family: "Monda", sans-serif;
}

input {
  width: 98%;
  height: 35px;
  background: transparent;
  border: none;
  border-bottom: 2px solid #025e1f;
  outline: none;
  margin-top: 5px;
}

input:nth-child(2) {
  margin-bottom: 20px;
}

label {
  font-weight: 700;
  font-size: 16px;
  color: rgba(6, 40, 61, 0.555);
}

.mot {
  color: rgba(6, 40, 61, 1);
  font-weight: 500;
  font-size: 15px;
}

.btn {
  font-size: 17px;
  background: #025e1f;
  border: none;
  width: 50%;
  border-radius: 10px;
  height: 40px;
  color: white;
  transition: all 0.3s;
  cursor: pointer;
  outline: none;
  font-weight: 700;
  margin-left: 50%;
  margin-top: 20px;
}
.btn:hover {
  background: white;
  color: #025e1f;
}
.pas {
  font-weight: 700;
  color: rgba(6, 40, 61, 1);
  font-size: 14px;
}

.pas a {
  text-decoration: none;
  color: rgb(214, 106, 5);
}

.loading-indicator::after {
  content: "";
  display: inline-block;
  width: 23px;
  height: 23px;
  border-radius: 50%;
  border: 3px solid #06283d;
  border-top-color: white;
  border-bottom-color: white;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.loading-indicator {
  display: flex;
  justify-content: center;
  height: 100px;
}
</style>
