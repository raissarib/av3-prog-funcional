<template>
  <div class="transacoes-outer-container">
    <div class="transacoes-container">
      <h3>Transações</h3>
      <div class="form-container">
        <form @submit.prevent="adicionarTransacao" class="form">
          <div class="form-group">
            <label for="valor">Valor:</label>
            <input
              type="text"
              id="valor"
              v-model="novaTransacao.valor"
              placeholder="Digite o valor..."
              required
            />
          </div>
          <div class="form-group">
            <label for="tipo">Tipo:</label>
            <select id="tipo" v-model="novaTransacao.tipo" required>
              <option value="" disabled>Selecione o tipo</option>
              <option value="credito">Crédito</option>
              <option value="debito">Débito</option>
            </select>
          </div>
          <button type="submit">Criar</button>
        </form>
      </div>
      <div class="transacoes-lista-horiz-scroll">
        <div class="transacoes-lista">
          <div
            class="transacao"
            v-for="transacao in transacoes"
            :key="transacao.id"
          >
            <div class="transacao-info">
              <div class="transacao-tipo">
                Tipo: {{ traduzirTipo(transacao.tipo) }}
              </div>
              <div class="transacao-valor">
                Valor: R$ {{ parseFloat(transacao.valor).toFixed(2) }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      transacoes: [],
      novaTransacao: {
        valor: "",
        tipo: "",
      },
    };
  },
  created() {
    this.getTransacoes();
  },
  methods: {
    getTransacoes() {
      axios
        .get("http://localhost:3000/transactions")
        .then((transactions) => {
          this.transacoes = transactions.data;
        })
        .catch((error) => {
          console.error("Erro ao buscar transações:", error);
        });
    },
    traduzirTipo(tipo) {
      return tipo === "credito"
        ? "Crédito"
        : tipo === "debito"
        ? "Débito"
        : tipo;
    },
    adicionarTransacao() {
      if (this.novaTransacao.valor && this.novaTransacao.tipo) {
        axios
          .post("http://localhost:3000/transactions", this.novaTransacao)
          .then(() => {
            this.getTransacoes();
          })
          .catch((error) => {
            console.error("Erro ao adicionar transação:", error);
          });

        this.novaTransacao.valor = "";
        this.novaTransacao.tipo = "";
      }
    },
  },
};
</script>

<style>
.transacoes-container h3 {
  text-align: center;
  text-transform: uppercase;
  margin-bottom: 30px;
  font-size: 20px;
}
.transacoes-outer-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.transacoes-container {
  width: 80%;
  max-width: 800px;
  border: 1px solid #ccc;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
  background-color: #fff;
  white-space: nowrap;
  display: flex;
  flex-direction: column;
}

.form-container {
  border-bottom: 1px solid #ccc;
  margin-bottom: 20px;
  padding-bottom: 20px;
  text-align: center;
}

.form {
  display: inline-block;
}

.form-group {
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
}

.form-group label {
  margin-bottom: 5px;
  flex: 1;
  color: #888;
}

.form-group input,
.form-group select {
  flex: 2;
  margin-right: 25px;
}

form input,
form select,
button {
  width: 100%;
  padding: 5px;
  font-size: 16px;
}
button {
  margin-top: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.transacoes-lista-horiz-scroll {
  overflow-x: auto;
}

.transacoes-lista {
  white-space: nowrap;
  display: inline-flex;
}

.transacao {
  padding: 20px;
  margin: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #fafafa;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.transacao-info {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.transacao div {
  margin-bottom: 5px;
}
</style>
