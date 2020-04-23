<template>
    <div>   
        <Titulo :texto="`Aluno: ${aluno.nome}`" :btnVoltar="!visualizando">
            <button v-show="visualizando" v-on:click="editar()" class="btn btnEditar">Editar</button>
        </Titulo>
        <table>
            <tbody>
                <tr>
                    <td class="colPequeno">Matrícula:</td>
                    <td>
                        <label>{{ aluno.id }}</label>
                    </td>
                </tr>
                <tr>
                    <td class="colPequeno">Nome:</td>
                    <td>
                        <label v-if="visualizando">{{ aluno.nome }}</label>
                        <input v-else v-model="aluno.nome" type="text">
                    </td>
                </tr>
                <tr>
                    <td class="colPequeno">Sobrenome:</td>
                    <td>
                        <label v-if="visualizando">{{ aluno.sobrenome }}</label>
                        <input v-else v-model="aluno.sobrenome" type="text">
                    </td>
                </tr>
                <tr>
                    <td class="colPequeno">Data de nascimento:</td>
                    <td>
                        <label v-if="visualizando">{{ aluno.dataNasc }}</label>
                        <input v-else v-model="aluno.dataNasc" type="text">
                    </td>
                </tr>
                <tr>
                    <td class="colPequeno">Professor:</td>
                    <td>
                        <label v-if="visualizando">{{ aluno.professor.nome }}</label>
                        <select v-model="aluno.professor" v-else>
                            <option v-for="(professor, index) in professores"
                            :key="index" v-bind:value="professor">{{ professor.nome }}</option>
                        </select>
                    </td>
                </tr>
            </tbody>
        </table>
        <div>
            <div style="margin-top: 10px">
                <div v-if="!visualizando">
                    <button class="btn btnSalvar" @click="salvar(aluno)">Salvar</button>
                    <button class="btn btnCancelar" @click="cancelar()">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Titulo from '../_share/Titulo'
export default {
    components: {
        Titulo
    },
    data() {
        return {
            professores: [],
            aluno: {},
            id: this.$route.params.id,
            visualizando: true
        }
    },
    created() {
        //fetch('http://localhost:3000/alunos/'+ this.id)
        fetch('https://my-json-server.typicode.com/jonathanafranio/curso-vue/alunos/'+ this.id)
        .then(res => res.json())
        .then(aluno => this.aluno = aluno);

        //fetch('http://localhost:3000/professores')
        fetch('https://my-json-server.typicode.com/jonathanafranio/curso-vue/professores')
        .then(res => res.json())
        .then(professor => this.professores = professor)
    },
    methods: {
        editar(){
            this.visualizando = !this.visualizando;
        },
        salvar(_aluno){
            let _alunoEditar = {
                id: _aluno.id,
                nome: _aluno.nome,
                sobrenome: _aluno.sobrenome,
                dataNasc: _aluno.dataNasc,
                professor: _aluno.professor
            };
            //fetch('http://localhost:3000/alunos/'+ _alunoEditar.id, {
            fetch('https://my-json-server.typicode.com/jonathanafranio/alunos/'+ _alunoEditar.id, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(_alunoEditar)
            });
            this.visualizando = !this.visualizando;
        },
        cancelar(){
            this.visualizando = !this.visualizando;
        }
    }
};
</script>
<style scoped>
.colPequeno {
    width: 20%;
    text-align: right;
    background-color: rgb(125, 217, 245);
    font-weight: 700;
}
.btnEditar {
    float: right;
    background: #054f77;
}
.btnSalvar {
    float: right;
    background: #28a745;
}
.btnCancelar {
    float: left;
    background-color: #dc3545;
}
</style>
