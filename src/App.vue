<template>
  <div>
    <h1 class="titulo">Digite seu CEP</h1>
    <input
      type="text"
      v-model="cep"
      placeholder="Digite seu CEP aqui"
      maxlength="8"
    />
    <ul>
      <li class="informacao" v-for="(valor, chave) in endereco" :key="chave">
        {{ chave }}: {{ valor }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cep: "",
      endereco: {},
    };
  },
  watch: {
    cep(valor) {
      if (valor.length === 8) {
        fetch(`https://viacep.com.br/ws/${valor}/json/`)
          .then((r) => {
            if (!r.ok) {
              throw new Error("Não foi possível obter os dados do CEP");
            }
            return r.json();
          })
          .then((r) => {
            this.endereco = r;
          })
          .catch((e) => {
            console.error(e);
          });
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Barlow:ital,wght@0,700;1,200&display=swap");
body {
  background-color: black;
}
div {
  font-family: "Barlow", sans-serif;
  letter-spacing: 0.25rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #333;
  margin: 1rem;
  border-radius: 0.5rem;
}
ul li {
  font-family: "Barlow", sans-serif;
  font-style: italic;
  padding: 0.25rem;
  letter-spacing: 0.1rem;
}
ul {
  border-radius: 0.5rem;
  padding: 0.5rem;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px,
    rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px,
    rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
}
.titulo {
  color: #fff;
}
.informacao {
  list-style: none;
  color: white;
  background-color: #000;
  margin: 0.25rem;
  border-radius: 0.2rem;
}
</style>
