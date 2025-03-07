<template>
  <div>
    <h2>Création de Profil</h2>
    <form @submit.prevent="submitForm">
      <div>
        <label for="roles">Choisissez des rôles :</label>
        <select multiple v-model="selectedRoles" @change="fetchPrivileges">
          <option v-for="role in roles" :key="role.id" :value="role.id">
            {{ role.name }}
          </option>
        </select>
      </div>

      <div v-if="privileges.length">
        <h3>Privilèges</h3>
        <label>
          <input
            type="checkbox"
            :checked="allPrivilegesSelected"
            @change="toggleAllPrivileges"
          />
          Tout sélectionner
        </label>
        <div v-for="privilege in privileges" :key="privilege.id">
          <label>
            <input type="checkbox" v-model="selectedPrivileges" :value="privilege.id" />
            {{ privilege.name }}
          </label>
        </div>
      </div>

      <button type="submit">Créer Profil</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      roles: [], // Liste des rôles
      privileges: [], // Privilèges des rôles sélectionnés
      selectedRoles: [], // Rôles sélectionnés
      selectedPrivileges: [], // Privilèges sélectionnés
      isLoading: false, // État de chargement
    };
  },
  computed: {
    allPrivilegesSelected() {
      return (
        this.privileges.length > 0 &&
        this.selectedPrivileges.length === this.privileges.length
      );
    },
  },
  mounted() {
    this.fetchRoles();
  },
  methods: {
    async fetchRoles() {
      this.isLoading = true; // Optionnel : état de chargement
      try {
        const token = localStorage.getItem("jwt");
        const response = await axios.get("http://192.168.1.132:8081/api/roles/all", {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        });
        this.roles = response.data;
        console.log(this.roles);
        
      } catch (error) {
        console.error("Erreur lors de la récupération des rôles:", error);
      } finally {
        this.isLoading = false; // Réinitialiser l'état de chargement
      }
    },
    async fetchPrivileges() {
      this.isLoading = true; // Optionnel : état de chargement
      try {
        const rolesString = this.selectedRoles.join(",");
        const token = localStorage.getItem("jwt");
        const response = await axios.get(`/api/roles/privileges?roles=${rolesString}`, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        });
        this.privileges = response.data;
        this.selectedPrivileges = []; // Réinitialiser les privilèges sélectionnés
      } catch (error) {
        console.error("Erreur lors de la récupération des privilèges:", error);
      } finally {
        this.isLoading = false; // Réinitialiser l'état de chargement
      }
    },
    toggleAllPrivileges() {
      if (this.allPrivilegesSelected) {
        this.selectedPrivileges = []; // Désélectionner tous
      } else {
        this.selectedPrivileges = this.privileges.map((privilege) => privilege.id); // Sélectionner tous
      }
    },
    async submitForm() {
      const profileData = {
        roles: this.selectedRoles,
        privileges: this.selectedPrivileges,
      };
      console.log("Profil à créer:", profileData);
      try {
        const token = localStorage.getItem("jwt");
        const response = await axios.post("/api/profiles", profileData, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        });
        console.log("Profil créé avec succès:", response.data);
      } catch (error) {
        console.error("Erreur lors de la création du profil:", error);
      }
    },
  },
};
</script>

<style scoped>
/* Ajoute du style selon tes besoins */
</style>
