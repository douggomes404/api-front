<template>
  <div id="app">

    <nav class="flex items-center justify-center flex-wrap bg-teal-500 p-6">
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="text-lg italic text-red-900">Front Busca calças</a>
      </div>
    </nav>

    <div class="center">

      <ul>
        <li v-for= "(erro, index) of errors" :key="index">
          campo <b>{{ erro.field }} </b> - {{ erro.defaultMessage }}
        </li>
      </ul>
    
        <div class="bg-white rounded shadow-2xl p-8 m-4">
          <form @submit.prevent="salvar" class="w-3/4 align-middle">
            <div class="flex">
              <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
                <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
                  Nome do Produto:
                </label>
                <input class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-500 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" type="text" placeholder="Nome" v-model="produto.nomeProduto">
              </div>
              <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
                <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
                  Quantidade:
                </label>
                <input class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-500 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" type="number" placeholder="Quantidade" v-model="produto.qntestoque">
              </div>
              <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
                <label class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2" for="grid-first-name">
                  Preço:
                </label>
              <input class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-500 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white" type="text" placeholder="Valor" v-model="produto.preco">
              </div>
              <div class="w-full md:w-2 px-3 mb-6 md:mb-0 mt-6">
              <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 border border-blue-700 rounded">Salvar</button>
              </div>
            </div>
          </form>
        </div>
      
      <table class="min-w-full">

        <thead>

          <tr>
            <th class="px-6 py-3 border-b-2 border-gray-300 text-left leading-4 text-blue-500 tracking-wider">NOME</th>
            <th class="px-6 py-3 border-b-2 border-gray-300 text-left text-sm leading-4 text-blue-500 tracking-wider">QTD</th>
            <th class="px-6 py-3 border-b-2 border-gray-300 text-left text-sm leading-4 text-blue-500 tracking-wider">VALOR</th>
            <th class="px-6 py-3 border-b-2 border-gray-300 text-left text-sm leading-4 text-blue-500 tracking-wider">OPÇÕES</th>
          </tr>

        </thead>

        <tbody class="bg-white">

          <tr v-for="produto of produtos" :key="produto.id">

            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-500">
              <div class="flex items-center">
                <div>
                  <div class="text-sm leading-5 text-gray-800">{{ produto.nomeProduto }}</div>
                </div>
              </div>
              </td>
            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-500">{{ produto.qntestoque }}</td>
            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-500">{{ produto.preco }}</td>
            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-500"> 
              <button @click="editar(produto)" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">Editar</button>
              <button @click="remover(produto)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-full ml-3">Deletar</button>
            </td>

          </tr>

        </tbody >
      
      </table>

    </div>

  </div>
</template>

<script>
  import Produto from './services/produtos'

  export default{
    data(){
      return{
        produto:{
          id: '',
          nomeProduto: '',
          qntestoque:'',
          preco: ''
        },
        produtos:[],
        errors: []
      }
    },

    mounted(){
      this.listar()
    },

    methods: {

      listar(){
        Produto.listar().then(resposta => {
        this.produtos = resposta.data
      })
      },

      salvar(){
        
        if(!this.produto.id){
          Produto.salvar(this.produto).then(resposta =>{
          this.produto = {}
          alert('Salvo com sucesso!')
          this.resposta = resposta.data
          this.listar()
          this.errors = []
        }).catch(e =>{
          console.log(e.response.data.errors)
          this.errors = e.response.data.errors
        })
        }else{
            Produto.salvar(this.produto).then(resposta =>{
          this.produto = {}
          alert('Atualizado com sucesso!')
          this.resposta = resposta.data
          this.listar()
          this.errors = []
        }).catch(e =>{
          console.log(e.response.data.errors)
          this.errors = e.response.data.errors
        })
        }        
      },

        editar(produto){
          this.produto = produto
        },

        remover(produto){
          
          if(confirm('Deseja excluir o produto???')){
            Produto.apagar(produto).then(resposta =>{
            this.resposta = resposta.data
            this.listar();
            this.errors = []
          }).catch(e =>{
            console.log(e.response.data.errors)
            this.errors = e.response.data.errors
          })
          } 
        }
    }
  }

</script>

<style >
  
</style>
