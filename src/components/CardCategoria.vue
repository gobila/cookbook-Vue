<script lang="ts">
import type ICategorias from "@/interfaces/ICategorias";
// esse propType ajuda na tipagem do proprio vueTS  deixando mais especifico e ajudando no auto complete
import type { PropType } from "vue";

export default {
  props: {
    // funciona de forma parecida ao que o propTypes faz em react porém dentro de um único obejto da propria prop
    categoria: { type: Object as PropType<ICategorias>, required: true },
  },
};
</script>

<template>
  {{ categoria.nome }}
  {{ categoria.imagem }}
</template>
