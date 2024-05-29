<template>
  <div id="app">
    <h1>Blockchain</h1>
    <div v-if="!isLoading">
      <div class="blockchain-container">
        <div v-for="(block, index) in blockchain" :key="index">
          <div class="block">
            <div class="content">
              <strong>Index:</strong> {{ block.index }}<br />
              <strong>Previous Hash:</strong> {{ block.previousHash }}<br />
              <strong>Hash:</strong> {{ block.hash }}<br />
              <strong>Nonce:</strong> {{ block.nonce }}<br />
              <strong>Timestamp:</strong> {{ block.timeStamp }}<br />
            </div>
            <i v-if="!isLastBlock(index)" class="fas fa-arrow-right arrow"></i>
          </div>
        </div>
      </div>
      <button class="mine-button" :disabled="mining" @click="mineBlock">
        Minerar
      </button>
      <h3 v-if="mining">Minerando o próximo bloco...</h3>
    </div>
    <div v-else>
      <h3>Carregando a blockchain...</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      blockchain: [],
      transactions: [],
      isLoading: false,
      mining: false,
    };
  },
  created() {
    this.fetchBlockchain();
    this.fetchTransactions();
  },
  methods: {
    async mineBlock() {
      this.mining = true;
      try {
        const response = await fetch("http://localhost:3000/blockchain", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({ transactions: this.transactions }),
        });

        if (response.ok) {
          this.fetchBlockchain();
          console.log("Bloco minerado com sucesso!");
          this.mining = false;
        } else {
          console.error("Falha ao minerar o bloco.");
        }
      } catch (error) {
        console.error("Erro ao processar a requisição:", error);
      }
    },
    async fetchBlockchain() {
      this.isLoading = true;
      try {
        const response = await fetch("http://localhost:3000/blockchain");
        if (response.ok) {
          const data = await response.json();
          this.blockchain = data;
        } else {
          console.error("Falha ao obter a lista blockchain.");
        }
      } catch (error) {
        console.error("Erro ao processar a requisição:", error);
      }
      this.isLoading = false;
    },
    async fetchTransactions() {
      try {
        const response = await fetch("http://localhost:3000/transactions");
        if (response.ok) {
          const data = await response.json();
          this.transactions = data;
        } else {
          console.error("Falha ao obter a lista blockchain.");
        }
      } catch (error) {
        console.error("Erro ao processar a requisição:", error);
      }
    },
    isLastBlock(index) {
      return index === this.blockchain.length - 1;
    },
  },
};
</script>

<style>
.block {
  height: 200px;
  background-color: white;
  border: 2px solid #00ff00;
  border-radius: 8px;
  padding: 20px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.content {
  text-align: left;
}

.arrow {
  position: absolute;
  right: -20px;
  top: 50%;
  transform: translateY(-50%);
  color: black;
}

.blockchain-container {
  display: flex;
  overflow-x: scroll;
  gap: 20px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.mine-button {
  margin-top: 20px;
  background-color: rgb(49, 146, 220);
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.modal {
  display: none;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
