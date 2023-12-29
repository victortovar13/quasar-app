<template>
  <q-page padding>
    <h4>Agregar Productos</h4>
    <q-form
      class="row q-col-gutter-md"
      @submit.prevent="procesarFormulario"
      @reset="reset"
      ref="myForm"
    >
      <div class="col-12 col-sm-6">
        <q-input
          label="Producto"
          v-model="producto"
          lazy-rules
          :rules="[
            (val) => (val && val.length > 0) || 'Por favor escribe algo ',
          ]"
        ></q-input>
      </div>
      <div class="col-12 col-sm-6">
        <q-select
          label="Prioridad"
          v-model="seleccion"
          :options="opciones"
          lazy-rules
          :rules="[
            (val) => (val && val.length > 0) || 'Por favor escribe algo ',
          ]"
        ></q-select>
      </div>
      <div class="col-12">
        <q-toggle label="Aceptar los términos" v-model="terminos"></q-toggle>
      </div>
      <div class="col-12">
        <q-btn label="Submit" color="secondary" type="submit" />
        <q-btn
          label="Reset"
          color="secondary"
          type="reset"
          outline
          class="q-ml-sm"
        />
      </div>
    </q-form>
    <pintar-datos :productos="productos" />
  </q-page>
</template>
<script>
import { ref } from "vue";
import PintarDatos from "src/components/PintarDatos.vue";
import { useQuasar } from "quasar";
export default {
  components: { PintarDatos },
  setup() {
    const $q = useQuasar();
    const myForm = ref(null);
    const producto = ref(null);
    const seleccion = ref(null);
    const terminos = ref(false);
    const opciones = ["maxima", "moderada", "minima"];
    const productos = ref([]);
    const procesarFormulario = () => {
      if (terminos.value === false) {
        $q.notify({
          color: "red-5",
          textColor: "white",
          icon: "warning",
          message: "Faltó los términos",
        });
      } else {
        $q.notify({
          color: "green-4",
          textColor: "white",
          icon: "cloud_done",
          message: "Datos procesados",
        });
        myForm.value.resetValidation();

        // Procesar formulario
        productos.value = [
          ...productos.value,
          {
            producto: producto.value,
            prioridad: seleccion.value,
          },
        ];
        reset();
      }
    };
    const reset = () => {
      producto.value = null;
      seleccion.value = null;
      terminos.value = false;
    };
    return {
      producto,
      seleccion,
      opciones,
      procesarFormulario,
      terminos,
      reset,
      myForm,
      productos,
    };
  },
};
</script>
