<template>
    <div>
        <titulo texto="Professores" :btnVoltar="false" />
        <table>
            <thead>
                <th>Cod.</th>
                <th>Nome</th>
                <th>Alunos</th>
            </thead>
            <tbody v-if="Professores.length">
                <tr v-for="(professor, index) in Professores" :key="index">
                    <td>{{ professor.id }}</td>
                    <!--<td>{{ index + 1 }}</td>-->
                    <!--<router-link v-bind:to="'/alunos/' + professor.id" tag="td" class="td-link">-->
                    <router-link :to="'/alunos/' + professor.id" tag="td" class="td-link">
                        {{ professor.nome }} {{ professor.sobrenome }}
                    </router-link>
                    <td>
                        {{ professor.qtdAlunos }}
                    </td>
                </tr>
            </tbody>
            <tfoot v-else>
                Nenhum professor encontrado.
            </tfoot>
        </table>
    </div>
</template>

<script>
import Titulo from '../_share/Titulo.vue';

export default {
    components: {
        Titulo
    },
    data() {
        return {
            Professores: [],
            Alunos: []
        }
    },
    created(){
        //fetch('http://localhost:3000/alunos')
        fetch('https://my-json-server.typicode.com/jonathanafranio/curso-vue/alunos')
        .then(res => res.json())
        .then(alunos => {
            this.Alunos = alunos;
            this.carregarProfessores();
        })
    },
    props: {},
    methods: {
        pegarQtdAlunosPorProfessores(){
            this.Professores.forEach((professor, index) => {
                professor = {
                    id: professor.id,
                    nome: professor.nome,
                    qtdAlunos: this.Alunos.filter(aluno =>
                        aluno.professor.id == professor.id
                    ).length
                }
                this.Professores[index] = professor;
            });
        },
        carregarProfessores(){
            //fetch('http://localhost:3000/professores')
            fetch('https://my-json-server.typicode.com/jonathanafranio/curso-vue/professores')
            .then(res => res.json())
            .then(professor => {
                this.Professores = professor;
                this.pegarQtdAlunosPorProfessores();
            })
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .td-link {
        cursor: pointer;
    }
</style>
