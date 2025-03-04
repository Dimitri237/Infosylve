<template>
  <div class="container monda-font ">
    <nav>
      <img src="" alt="" />
    </nav>
    <div>
      <h2 class="monda-font">Infosylve Connexion</h2>
    </div>
    <form @submit.prevent="login">
      <div class="input-field">
        <div><label for="email">Téléphone/Email</label></div>
        <input id="contact" v-model="email" required>
      </div>
      <div class="input-field">
        <div><label for="password">Mot de passe</label></div>
        <input type="password" id="password" v-model="password" required>
      </div>
      <div>
        <button class="btn" type="submit" :disabled="loading">
          <span class="loading-indicator" v-if="loading"></span>
          <span v-else>Connexion</span>
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      email: '',
      password: '',
      loading: false,
    };
  },

  methods: {
    async login() {
      this.loading = true;
      try {
        const response = await axios.post('https://minsante-api-636b67309a26.herokuapp.com/login', {
          email: this.email,
          password: this.password,
        });

        const { token, userId, userName } = response.data;

        // Stocker l'ID de l'utilisateur dans le localStorage
        localStorage.setItem('userId', userId);
        localStorage.setItem('userName', userName);

        // Stocker le jeton d'authentification dans une variable ou le localStorage
        this.token = token;

        // Rediriger l'utilisateur vers une page protégée ou effectuer d'autres actions
        this.$router.push('/acceuilPage');
      } catch (error) {
        console.error(error);
        // Afficher un message d'erreur à l'utilisateur
      }finally {
        this.loading = false;
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
  background-color: rgba(255, 255, 255, 0.743);
  border-radius: 10px;
  text-align: left;
  width: 450px;
  height: 400px;
  margin: auto;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2) !important;
  padding: 15px;
  margin-top: 150px;
}

form {
  width: 100%;
  margin-top: 20px;
}

.monda-font {
  font-family: 'Monda', sans-serif;
}

input {
  width: 98%;
  height: 35px;
  background: transparent;
  border-radius: 10px;
  border: 2px solid #025e1f;
  outline: none;
  margin-top: 5px;

}

input:nth-child(2) {
  margin-bottom: 20px;
}

label {
  font-weight: 700;
  font-size: 16px;
  color: rgba(6, 40, 61, 0.555)
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
  border: 3px solid #06283D;
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