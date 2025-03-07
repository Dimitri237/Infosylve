<template>
<div>
    <ul>
        <li><router-link to="/userListe">Liste des utilisateurs</router-link></li>
        <li><router-link to="/optionList">Ajouter un itilisateur</router-link></li>
        <li><router-link to="/acceuil">parametres</router-link></li>
    </ul>
</div>
</template>

<script>
export default {
    data() {
        return {
        }
    },
    methods: {

    }
}
</script>
<style scoped>
main {
    width: 100;
    display: flex;
}

.sideBar {
    width: 15%;
    height: 100vh;
    background-color: white;
}

.container {
    width: 85%;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.1);
    border-radius: 15px;

}

section {
    width: 100;
}

.s1 {
    height: 59vh;
    display: flex;
    justify-content: space-between;
    padding: 0 20px;
    padding-top: 20px
}

.d1 {
    width: 33%;
    background-color: black;
    height: 55vh;
    border-radius: 15px;
}

.d2 {
    width: 65%;
    background-color: rgb(64, 142, 237);
    height: 55vh;
    border-radius: 15px;
}

.s2 {
    height: 39vh;
    padding: 0 20px;
}

.dive {
    width: 100%;
    background-color: rgb(246, 178, 178);
    height: 55vh;
    border-radius: 15px;
    height: 36vh;
}

.logo {
    font-weight: bold;
    font-size: 60px;
    font-family: Georgia, 'Times New Roman', Times, serif;
    width: 80%;
    text-align: center;
    margin-top: 65px;
    margin: auto;
}

.profile {
    width: 80%;
    margin: auto;
    margin-top: 150px !important;
    justify-content: center;
    align-items: center;
    text-align: center;
}

.profile img {
    width: 200px;
    margin: auto;
    height: 200px;
    border-radius: 50%;
    background-color: rgb(184, 182, 182);
}

.profile p {
    font-size: 25px;
    font-family: cursive;
    margin: auto;
    font-weight: bold;
}
</style>