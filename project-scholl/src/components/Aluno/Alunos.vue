<template>
    <div>
        <!--<titulo texto="Alunos" />-->
        <titulo :texto="professorid != undefined ? 'Professor: '+professor.nome : 'Todos os alunos'" :btnVoltar="false" />
        <div v-if="professorid != undefined" class="">
            <input type="text" placeholder="Nome do aluno" v-model="nome" v-on:keyup.enter="addAluno()">
            <button class="btn btnInput" @click="addAluno()">Adicionar</button>
        </div>

        <table>
            <thead>
                <th>Mat.</th>
                <th>Nome</th>
                <th>Opções</th>
            </thead>
            <tbody v-if="alunos.length">
                <tr v-for="(aluno, index) in alunos" :key="index">
                    <td>{{ aluno.id }}</td>
                    <!--<td>{{ index + 1 }}</td>-->
                    <router-link :to="`/alunoDetalhe/${aluno.id}`" tag="td" class="td-link">
                        {{ aluno.nome }} {{ aluno.sobrenome }}
                    </router-link>
                    <td>
                        <button class="btn btn-Danger" @click="remover(aluno)">Remover</button>
                        <!--
                        <button class="btn btn-Danger" @click="remover(index)">Remover</button>-->
                    </td>
                </tr>
            </tbody>
            <tfoot v-else>
                Nenhum aluno encontrado.
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
            titulo: "Aluno",
            professorid: this.$route.params.prof_id,
            professor: {},
            nome: "",
            alunos: [],
            /*alunos: [
                { id: 1, nome: 'Marcus', sobrenome: 'Xavier' },
                { id: 2, nome: 'Jonas', sobrenome: 'Silva' },
                { id: 3, nome: 'Paulo', sobrenome: 'Andre' }
            ]*/
        }
    },
    created(){
        let request = 'http://localhost:3000/alunos';
        //this.$http.get('http://localhost:3000/alunos')
        // Prefiro usar fetch do que o $http
        //O fetch ja faz a mesma coisa que o $http
        //...e nao precisa de instalar Vue Resource
        //...pq o fetch ja e nativo do proprio JS
        if(this.professorid) {
            request = 'http://localhost:3000/alunos/?professor.id='+this.professorid;
            this.carregarProfessores();
        }
        fetch(request)
        .then(res => res.json())
        .then(alunos => this.alunos = alunos);
    },
    props: {},
    methods: {
        addAluno() {
            if(this.nome==='') {
                return;
            }

            let _aluno = {
                nome: this.nome,
                sobrenome: "",
                professor: {
                    id: this.professor.id,
                    nome: this.professor.nome
                }
            };

            //this.$http.post('http://localhost:3000/alunos', _aluno)
            // Prefiro usar fetch do que o $http
            //O fetch ja faz a mesma coisa que o $http
            //...e nao precisa de instalar Vue Resource
            //...pq o fetch ja e nativo do proprio JS
            fetch('http://localhost:3000/alunos',{
                method: 'POST',
                headers: {
                    //'Accept': 'application/json', //nem precisou
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(_aluno)
            })
            .then(res => res.json())
            .then(aluno => {
                this.alunos.push(aluno);
                this.nome = '';
            });
        },
        remover(aluno) {
            //this.$http.delete(`http://localhost:3000/alunos/${aluno.id}`)
            // Prefiro usar fetch do que o $http
            //O fetch ja faz a mesma coisa que o $http
            //...e nao precisa de instalar Vue Resource
            //...pq o fetch ja e nativo do proprio JS
            fetch('http://localhost:3000/alunos/'+aluno.id,{
                method: 'DELETE'
            })
            .then(() => {
                let indice = this.alunos.indexOf(aluno);
                this.alunos.splice(indice, 1);
            });
        },
        carregarProfessores(){
            fetch('http://localhost:3000/professores/' + this.professorid)
            .then(res => res.json())
            .then(professor => {
                this.professor = professor;
            })
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
    width: calc(100% - 200px);
    padding: 20px;
    color: #687f7f;
    border: none;
    font-size: 1.3em;
    display: inline;
}
.btnInput {
    display: inline;
    border: none;
    padding: 20px;
    color: #fff;
    font-size: 1.3em;
    background-color: rgb(116, 115, 115);
}
.td-link {
    cursor: pointer;
}
</style>
