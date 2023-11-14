<template>
    <div class="modal" id="update-user-modal">
      <div class="r-modal-content">
        <div class="modal-header d-flex align-items-baseline">
          <h4>Edição de empresa</h4>
          <span class="close" @click="close">&times;</span>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-12">
              <div class="form-group">
                <label for="nome">Nome</label>
                <input type="text" class="form-control" id="nome" :value="nome" @input="updateNome" />
              </div>
            </div>
          </div>
  
          <div class="row">
            <div class="col-12">
              <div class="form-group">
                <label for="telefone">Ramo</label>
                <input type="text" class="form-control" id="telefone" :value="telefone" @input="updateTelefone" />
              </div>
            </div>
          </div>
          <div class="row mt-4">
            <div class="col">
              <button type="button" @click="save" class="btn btn-success">Salvar</button>
              <button type="button" @click="close" class="btn btn-link r-ml-2">Cancelar</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  
  <script>
  export default {
    props: {
      nome: String,
      email: String,
      telefone: String,
      cpf: String,
      funcao: String,
      showModal: Boolean,
      errorMessage: String,
      status: Boolean,
      userId: Number,
    },
    mounted() {
      this.initModal(); // Execute a inicialização do modal após a renderização do DOM
    },
    methods: {
      initModal() {
        const modal = document.getElementById("update-user-modal");
        if (modal) {
          modal.style.display = "none";
        }
      },
      close() {
        const modal = document.getElementById("update-user-modal");
        if (modal) {
          modal.style.display = "none";
        }
        this.clear(); // Chama o método clear sem passar o evento
      },
      clear() {
        // Implemente a lógica para limpar os campos ou fazer outras ações necessárias.
      },
      async save(updatedUser) {
        try {
          await http.put(`/usuario/${this.editUserId}`, updatedUser);
  
          const userIndex = this.usuarios.findIndex((u) => u.id === this.editUserId);
          if (userIndex !== -1) {
            this.usuarios[userIndex] = updatedUser;
          }
  
          this.closeUpdateModal();
        } catch (error) {
          alert('Erro ao atualizar o usuário. Tente novamente mais tarde.');
        }
      },
    },
  };
  </script>