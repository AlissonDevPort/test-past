<template>
  <div class="img">
    <img src="../assets/images/Logo.svg" alt="" />
  </div>
  <div class="pedido-form">
    <div class="teste">
      <section class="primary-header">
        <h2>Monte aqui o seu cardápio. O que está esperando?</h2>
        <div class="btn-checker">
          <p>Comida</p>
          <div
            class="toggle-switch"
            :class="{ checked: isChecked }"
            @click="toggle"
          >
            <div class="toggle-handle"></div>
          </div>
          <p>Bebida</p>
        </div>
      </section>
      <form @submit.prevent="submitForm">
        <section class="first-line-btn">
          <div class="form-row">
            <input
              id="titulo"
              type="text"
              v-model="titulo"
              placeholder="Titulo"
              required
            />
          </div>
          <div class="form-row">
            <input
              id="sabor"
              type="text"
              v-model="sabor"
              placeholder="Sabor"
              required
            />
          </div>

          <div class="form-row">
            <input
              id="preco"
              v-model.number="preco"
              type="number"
              placeholder="R$"
              required
            />
          </div>
        </section>
        <div class="second-line-btn">
          <input
            id="description"
            v-model="description"
            type="text"
            placeholder="Descrição"
          />
        </div>
        <section class="image-holder">
          <div class="upload-container" @click="selecionarArquivo">
            <img
              v-if="imagemPreview"
              :src="imagemPreview"
              alt="Imagem Preview"
              class="preview-imagem"
            />
            <p>Clique para escolher uma imagem</p>
            <input
              type="file"
              ref="inputFile"
              style="display: none"
              accept="image/*"
              @change="carregarImagem"
            />
          </div>
        </section>
        <div>
          <p v-if="!isFormValido" class="mensagem-erro">
            Por favor, preencha todos os campos obrigatórios.
          </p>
        </div>
        <div class="btns-holder">
          <button type="button" @click="limparFormulario" class="clean-btn">
            Limpar
          </button>
          <button type="submit" :disabled="!isFormValido" class="send-btn">
            Enviar
          </button>
        </div>
      </form>
    </div>
    <div class="ul-container">
      <ul class="lista-pedidos">
        <li
          v-for="(pedido, index) in listaPedidos"
          :key="index"
          class="pedido-item"
        >
          <div class="pedido-info">
            <div class="pedido-info-topo">
              <h2 class="titulo-ul">{{ pedido.titulo }}</h2>
              <p class="preco-ul">R$ {{ pedido.preco }}</p>
            </div>
            <p class="sabor-ul">Sabor: {{ pedido.sabor }}</p>
            <p class="sabor-ul">Descrição: {{ pedido.description }}</p>
          </div>
          <img
            :src="pedido.imagem || imagemPadrao"
            alt="Imagem Pedido"
            class="imagem-pedido"
          />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imagemPadrao: require("../assets/images/images@2x.png"),
      titulo: "",
      sabor: "",
      description: "",
      preco: null,
      nomeImagem: "",
      imagemPreview: null,
      listaPedidos: [{ titulo: "titulo", sabor: "Goiaba", preco: 33 }],
      isChecked: false,
    };
  },
  computed: {
    isTituloValido() {
      return this.titulo.length >= 3 && this.titulo.length <= 60;
    },
    isSaborValido() {
      return this.sabor.length >= 3 && this.sabor.length <= 60;
    },
    isPrecoValido() {
      return this.preco >= 1;
    },
    isFormValido() {
      return this.isTituloValido && this.isSaborValido && this.isPrecoValido;
    },
  },
  methods: {
    selecionarArquivo() {
      if (this.$refs.inputFile) {
        this.$refs.inputFile.click();
      } else {
        console.error("Referência para inputFile não encontrada.");
      }
    },
    carregarImagem(event) {
      const arquivo = event.target.files[0];
      if (arquivo) {
        const reader = new FileReader();
        reader.onload = () => {
          this.imagemPreview = reader.result;
        };
        reader.readAsDataURL(arquivo);
      } else {
        this.imagemPreview = null;
      }
    },
    toggle() {
      this.isChecked = !this.isChecked;
    },
    submitForm() {
      if (this.isFormValido) {
        const novoPedido = {
          titulo: this.titulo,
          sabor: this.sabor,
          preco: this.preco,
          description: this.description,
          imagem: this.imagemPreview,
        };
        this.listaPedidos.push(novoPedido);
        this.limparFormulario();
      }
      this.nomeImagem = "";
    },
    limparFormulario() {
      this.titulo = "";
      this.sabor = "";
      this.description = "";
      this.preco = null;
      this.nomeImagem;
      this.imagemPreview = null;
    },
  },
};
</script>

<style scoped>
.ul-container {
  margin-top: 60px;
}

h2 {
  font-style: italic;
}
.second-line-btn {
  padding: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: white;
}
#description {
  flex-grow: 1;
  height: 45px;
  border-radius: 8px;
  border: 1px solid red;
}
.teste {
  border-radius: 20px;
  box-shadow: rgba(0, 0, 0, 0.8) 0 0 10px;
  border-collapse: collapse;
  width: 60vw;
  height: 40vh;
  @media screen and (max-width: 375px) {
    width: 90vw;
    height: 50vh;
  }
  @media screen and (min-width: 375px) and (max-width: 580px) {
    width: 90vw;
    height: 40vh;
  }
}
img {
  height: 170px;
  @media screen and (max-width: 750px) {
    width: 80vw;
  }
}
.img {
  display: flex;
  justify-content: center;
  padding: 50px;
}
.pedido-form {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.lista-pedidos {
  list-style-type: none;
  padding: 0;
  width: 75vw;
  @media screen and (max-width: 580px) {
    width: 85vw;
  }
}

.pedido-item {
  border-radius: 20px;
  box-shadow: rgba(0, 0, 0, 0.8) 0 0 10px;
  border-collapse: collapse;
  margin-bottom: 10px;
  position: relative;
}

.pedido-info {
  display: flex;
  flex-direction: column;
}

.pedido-info-topo {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 5px;
  background-color: #e43636;
  display: flex;
  justify-content: space-between;

  border-radius: 20px 20px 0 0;

  font-size: 11px;
}
.titulo-ul {
  color: #ffca00;
  font-style: italic;
}

.titulo {
  font-weight: bold;
}

.preview-imagem {
  width: 100px;
  height: 40px;
}
.imagem-pedido {
  width: 50px;
  height: 50px;
  margin-top: 5px;
}
.first-line-btn {
  background-color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
}
.btn-checker {
  display: flex;
  justify-content: center;
  align-items: center;
}
.toggle-switch {
  width: 50px;
  height: 30px;
  background-color: rgb(255, 255, 255);
  border-radius: 15px;
  position: relative;
  cursor: pointer;
}

.toggle-handle {
  width: 24px;
  height: 24px;
  background-color: rgb(194, 23, 23);
  border-radius: 50%;
  position: absolute;
  top: 3px;
  left: 3px;
  transition: left 0.3s ease-in-out;
}

.toggle-switch.checked .toggle-handle {
  left: calc(100% - 27px);
}
.primary-header {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  background: #ffca00;
  border-radius: 20px 20px 0 0;
  color: #a13600;
  font-size: 11px;
}
.upload-container {
  border: 1px solid red;
  padding: 20px;
  text-align: center;
  cursor: pointer;
  border-radius: 8px;
}
.mensagem-erro {
  display: flex;
  justify-content: center;
  font-size: 10px;
  color: red;
}
.upload-icon {
  width: 50px;
  height: 50px;
  margin-bottom: 10px;
}
.form-row {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.form-row input {
  flex-grow: 1;
  height: 25px;
  border-radius: 8px;
  border: 1px solid red;
  @media screen and (max-width: 380px) {
    width: 100px;
  }
  @media screen and (min-width: 380px) and (max-width: 880px) {
    width: 120px;
  }
}
.send-btn {
  height: 45px;
  font-size: 16px;
  font-weight: bold;
  width: 90px;
  border: none;
  border-radius: 20px;
  background-color: #ffca00;
  color: #a13600;
}
.sabor-ul {
  margin-left: 40px;
  word-wrap: break-word;
}
.clean-btn {
  height: 45px;
  font-size: 16px;
  font-weight: bold;
  width: 90px;
  border: none;
  border-radius: 20px;
  margin-right: 10px;
  color: white;
  background-color: #e43636;
}

#preco {
  flex-grow: 0.5;
  @media screen and (max-width: 380px) {
    width: 50px;
  }
  @media screen and (min-width: 380px) and (max-width: 880px) {
    width: 80px;
  }
}
::placeholder {
  color: #a13600;
}
.image-holder {
  padding: 8px;
}
.btns-holder {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  justify-content: center;
  align-items: center;
}
.imagem-pedido {
  width: 50px;
  height: 50px;
  margin-top: 5px;
  position: absolute;
  top: 75px;
  left: -25px;
}
</style>
