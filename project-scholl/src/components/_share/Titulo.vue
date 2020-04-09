<template>
    <div>
        <h2>{{ texto }}</h2>
        <div class="voltar">
            <button v-show="btnVoltar" class="btn btnVoltar" @click="voltar()">Voltar</button>
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        texto: String,
        btnVoltar: Boolean
    },
    methods: {
        voltar() {
            this.$router.back();
        }
    }
}
</script>

<style scoped>
h2 {
    width: calc(100% - 180px);
}
.voltar {
    width: 170%;
    position: absolute;
    right: 0;
    top: 0;
    /*margin-top: 15px;*/
}
.btnVoltar {
    float: right;
    font-size: 0.8rem;
    background: #ccc;
    color: #000;
}
</style>
