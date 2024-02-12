<script lang="ts">
import MostrarReceitas from "./MostrarReceitas.vue";
import SelecionarIngredientes from "./SelecionarIngredientes.vue";
import Tag from "./Tag.vue";

type Pagina = "SelecionarIngredientes" | "MostrarReceitas";
export default {
  data() {
    return {
      ingredientes: [] as string[],
      conteudo: "SelecionarIngredientes" as Pagina,
    };
  },
  components: { SelecionarIngredientes, Tag, MostrarReceitas },
  methods: {
    adicionarIngredientes(ingrediente: string) {
      this.ingredientes.push(ingrediente);
    },
    removerIngredientes(ingrediente: string) {
      this.ingredientes = this.ingredientes.filter(
        (iLista) => ingrediente !== iLista
      );
    },
    navegar(pagina: Pagina) {
      this.conteudo = pagina;
    },
  },
};
</script>

<template>
  <main class="conteudo-principal">
    <section>
      <span class="subtitulo-lg sua-lista-texto"> Sua lista: </span>
      <ul v-if="ingredientes.length > 0" class="ingredientes-sua-lista">
        <li v-for="ingrediente in ingredientes" v-bind:key="ingrediente">
          <Tag :texto="ingrediente" ativa />
        </li>
      </ul>

      <p v-else class="paragrafo lista-vazia">
        <img
          src="../assets/imagens/icones/lista-vazia.svg"
          alt="Ícone de pesquisa"
        />
        Sua lista está vazia, selecione ingredientes para iniciar.
      </p>
    </section>
    <!-- KeepAlive serve para preservar o estado do componente que estao dentro dele atraves de cache, o
    parametro include serve para cacher apenas o que se quer fazendo um filtro dentro de tudo q esta dentro do KeepAlive
    é necessario adicionar a propriedade name no componente que está no include -->
    <!-- Aqui em SelecionarIngredientes o $event esta pegando o dado do evento que é o ingrediente, meio que funcionando como o event.target  -->
    <KeepAlive include="SelecionarIngredientes">
      <SelecionarIngredientes
        v-if="conteudo === 'SelecionarIngredientes'"
        @adicionar-ingrediente="adicionarIngredientes($event)"
        @remover-ingrediente="removerIngredientes($event)"
        @buscar-receitas="navegar('MostrarReceitas')"
      />
      <MostrarReceitas
        v-else-if="conteudo === 'MostrarReceitas'"
        :ingredientes="ingredientes"
        @editar-receitas="navegar('SelecionarIngredientes')"
      />
    </KeepAlive>
  </main>
</template>

<style scoped>
.conteudo-principal {
  padding: 6.5rem 7.5rem;
  border-radius: 3.75rem 3.75rem 0rem 0rem;
  background: var(--creme, #fffaf3);
  color: var(--cinza, #444);

  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5rem;
}

.sua-lista-texto {
  color: var(--coral, #f0633c);
  display: block;
  text-align: center;
  margin-bottom: 1.5rem;
}

.ingredientes-sua-lista {
  display: flex;
  justify-content: center;
  gap: 1rem 1.5rem;
  flex-wrap: wrap;
}

.lista-vazia {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.25rem;

  color: var(--coral, #f0633c);
  text-align: center;
}

@media only screen and (max-width: 1300px) {
  .conteudo-principal {
    padding: 5rem 3.75rem;
    gap: 3.5rem;
  }
}

@media only screen and (max-width: 767px) {
  .conteudo-principal {
    padding: 4rem 1.5rem;
    gap: 4rem;
  }
}
</style>
