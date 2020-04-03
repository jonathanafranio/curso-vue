<template>
    <div>
        <titulo texto="Alunos" />
        <div class="">
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
                    <td>{{ aluno.nome }} {{ aluno.sobrenome }}</td>
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
            nome: "",
            alunos: []
            /*alunos: [
                { id: 1, nome: 'Marcus', sobrenome: 'Xavier' },
                { id: 2, nome: 'Jonas', sobrenome: 'Silva' },
                { id: 3, nome: 'Paulo', sobrenome: 'Andre' }
            ]*/
        }
    },
    created(){
        //this.$http.get('http://localhost:3000/alunos')
        // Prefiro usar fetch do que o $http
        //O fetch ja faz a mesma coisa que o $http
        //...e nao precisa de instalar Vue Resource
        //...pq o fetch ja e nativo do proprio JS
        fetch('http://localhost:3000/alunos')
        .then(res => res.json())
        .then(alunos => this.alunos = alunos)
    },
    props: {
    },
    methods: {
        addAluno() {
            let _aluno = {
                nome: this.nome,
                sobrenome: ""
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
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
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
</style>
