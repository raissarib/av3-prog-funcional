<template>
  <div>
    <nav class="navbar">
      <button @click="showTransactions" class="nav-button">Transações</button>
      <button @click="showBlockchain" class="nav-button">Blockchain</button>
    </nav>
    <div v-if="currentRoute === 'transactions'">
      <TransactionsItens />
    </div>
    <div v-else-if="currentRoute === 'blockchain'">
      <BlockchainItens />
    </div>
  </div>
</template>

<script>
import TransactionsItens from "./components/TransactionsItens.vue";
import BlockchainItens from "./components/BlockchainItens.vue";

export default {
  name: "App",
  components: {
    TransactionsItens,
    BlockchainItens,
  },
  data() {
    return {
      currentRoute: "transactions",
    };
  },
  methods: {
    showTransactions() {
      this.currentRoute = "transactions";
    },
    showBlockchain() {
      this.currentRoute = "blockchain";
    },
  },
};
</script>

<style>
.nav-button {
  margin-right: 10px;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.nav-button:focus {
  outline: none;
}

.navbar {
  margin-bottom: 20px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

.navbar-narrow {
  width: 50%;
}
</style>
