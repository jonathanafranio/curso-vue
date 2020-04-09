<template>
    <div class="barraTop">
        <div class="marginPrincipal">
            <nav>
                <router-link to="/professores">Professores</router-link>
                <router-link to="/todosalunos">Alunos</router-link>
                <router-link to="/sobre">Sobre</router-link>
            </nav>
        </div>
    </div>
</template>
<script>
export default {

}
</script>
<style scoped>
.barraTop {
    background-color: #e6e6e6;
}
nav {
    padding: 20px 20px 20px 0px;
    width: 100%;
    display: flex;
    justify-content: space-between;
}
nav a {
    padding: 10px;
    text-decoration: none;
    background-color: #fff;
    color: #006eff;
    font-weight: 700;
    margin-right: 15px;
}
nav .router-link-active {
    background-color: #e6e6e6;
}
</style>
