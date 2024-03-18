<template>
  <div class="pagina-inicial">
    <PedidoForm @novoPedido="adicionarPedido" />
  </div>
</template>

<script>
import PedidoForm from "@/components/PedidoForm.vue";
//import PedidosLista from "@/components/PedidosLista.vue";

export default {
  components: {
    PedidoForm,
    // PedidosLista,
  },
};
</script>

<style scoped>
.pagina-inicial {
  height: 100vh;
}
</style>
