<template>
  <div class="home">
    <div class="section" id="Empresa">
      <button @click="show = !show" class="section-button">
        Empresa
      </button>
      <transition name="fade">
        <div v-if="show">
          <p>
            Nome: <input v-model="nome" type="text" class="input-field" name="nome" id="nome" />
            Ramo: <input v-model="ramo" type="text" class="input-field" name="ramo" id="ramo" />
            <button @click="createEmpresa" class="btn-cadastrar">Cadastrar</button>
            <button @click="getEmpresa" class="btn-cadastrar">Mostrar empresas cadastradas</button>
          </p>

          <div v-if="empresas.length > 0">
            <h2>Empresas Cadastradas:</h2>
            <table>
              <thead>
                <tr>
                  <th>Nome</th>
                  <th>Ramo</th>
                  <th>Ações</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(empresa, index) in empresas" :key="index">
                  <td>{{ empresa.nome }}</td>
                  <td>{{ empresa.ramo }}</td>
                  <td>
                    <button @click="editarEmpresa(index)" class="btn-editar"><i class="fas fa-pencil-alt"></i> Editar</button>
                    <button @click="deletarEmpresa(index)" class="btn-deletar"><i class="fas fa-trash-alt"></i> Deletar</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </transition>
    </div>

    <div class="section" id="Funcionario">
      <button @click="showFuncionario = !showFuncionario" class="section-button">
        Funcionario
      </button>
      <transition name="fade">
        <div v-if="showFuncionario">
          <p>
            Id: <input type="number" class="input-field" name="id" id="id" v-model="funcionarioId" />
            Name: <input type="text" class="input-field" name="name" id="name" v-model="funcionarioName" />
            <button @click="cadastrarFuncionario" class="btn-cadastrar">Cadastrar</button>
            <button @click="getFuncionario" class="btn-cadastrar">Mostrar Funcionarios</button>
            <h2>Funcionarios Cadastrados:</h2>
            <table>
              <thead>
                <tr>
                  <th>Nome</th>
                  <th>Ações</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(funcionario, index) in funcionarios" :key="index">
                  <td>{{ funcionario }}</td>
                  <td>
                    <button @click="editarFuncionario(index)" class="btn-editar"><i class="fas fa-pencil-alt"></i> Editar</button>
                    <button @click="deletarFuncionario(index)" class="btn-deletar"><i class="fas fa-trash-alt"></i> Deletar</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </p>
        </div>
      </transition>
    </div>

    <div class="section" id="Equipe">
      <button @click="showEquipe = !showEquipe" class="section-button">
        Equipe
      </button>
      <transition name="fade">
        <div v-if="showEquipe">
          <p>
            Cadastrar lider: <input v-model="liderNome" type="text" class="input-field" name="nome" id="nome" />
            <button @click="cadastrarLider" class="btn-cadastrar">Cadastrar</button>
          </p>
          <div v-if="funcionarios.length > 0">
            <h2>Funcionários Cadastrados:</h2>
            <table>
              <thead>
                <tr>
                  <th>Nomes</th>
                  <th>Ações</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(funcionario, index) in funcionarios" :key="index">
                  <td>{{ funcionario }}</td>
                  <td>
                    <button @click="editarFuncionario(index)" class="btn-editar"><i class="fas fa-pencil-alt"></i> Editar</button>
                    <button @click="deletarFuncionario(index)" class="btn-deletar"><i class="fas fa-trash-alt"></i> Deletar</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import http from '@/services/http';

export default defineComponent({
  data() {
    return {
      show: false,
      showFuncionario: false,
      showEquipe: false,
      funcionarios: [] as string[],
      empresas: [] as string[],
      nome: '',
      ramo: '',
      funcionarioId: 0,
      funcionarioName: '',
      liderNome: '',
    };
  },
  methods: {
    async createEmpresa() {
      const empresa = {
        nome: this.nome,
        ramo: this.ramo,
      };
      try {
        const response = await http.post('/empresa', empresa);
        this.empresas.push(response.data);
        this.nome = '';
        this.ramo = '';
      } catch (error) {
        console.error('Erro ao cadastrar empresa:', error);
      }
    },
    async getEmpresa() {
      http.get('/empresa').then((x: any) => (this.empresas = x.data)).catch((err) =>
        alert('Algo deu errado, tente novamente mais tarde.')
      );
    },
    async getFuncionario() {
      http.get('/funcionarios').then((x: any) => (this.funcionarios = x.data)).catch((err) =>
        alert('Algo deu errado, tente novamente mais tarde.')
      );
    },

    async cadastrarFuncionario() {
      const funcionario = {
        id: this.funcionarioId,
        name: this.funcionarioName,
      };
      try {
        const response = await http.post('/funcionarios', funcionario);
        this.funcionarioId = 0;
        this.funcionarioName = '';
      } catch (error) {
        console.log(error);
      }
    },
    editarEmpresa(index: number) {
      // Lógica para editar a empresa
      console.log('Editar Empresa:', index);
    },
    deletarEmpresa(index: number) {
      // Lógica para deletar a empresa
      console.log('Deletar Empresa:', index);
    },
    async cadastrarLider() {
      const lider = {
        nome: this.liderNome,
      };

      const response = await http.post('/lider', lider);
      this.liderNome = '';
    },
    // ... (outros métodos de edição e exclusão)
  },
});
</script>

<style scoped>
/* Estilos gerais */
.home {
  background-color: #f2f2f2;
  padding: 20px;
}

.section {
  background-color: #fff;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.section-button {
  background-color: #3498db;
  color: #fff;
  border: none;
  padding: 12px 24px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 18px;
  cursor: pointer;
  border-radius: 6px;
  transition: background-color 0.3s;
  margin-bottom: 10px;
}

.section-button:hover {
  background-color: #2980b9;
}

.input-field {
  margin-bottom: 10px;
  padding: 8px;
  width: 100%;
  box-sizing: border-box;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #4caf50;
  color: white;
}

.btn-cadastrar,
.btn-editar,
.btn-deletar {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
  margin-left: 10px;
}

.btn-cadastrar:hover,
.btn-editar:hover,
.btn-deletar:hover {
  background-color: #45a049;
}
</style>
