
<script setup>
import { onMounted, reactive } from "vue";
import { FormKit } from "@formkit/vue";
import { useRouter, useRoute } from "vue-router";
import RouterLink from "../components/UI/RouterLInk.vue";
import Heading from "../components/UI/Heading.vue";
import ClienteService from "../services/ClienteService";

const router = useRouter();
const route = useRoute();

const { id } = route.params;

const formDta = reactive({});

onMounted(() => {
  ClienteService.obtenerCliente(id)
    .then(({ data }) => {
      Object.assign(formDta, data);
    })
    .catch((error) => console.log(error));
});

defineProps({
  titulo: {
    type: String,
  },
});
const handleSubmit = (data) => {
  ClienteService.actualizarCliente(id,data)
    .then(()=> router.push({name:'listado-clientes'}))
    .catch(error => console.log(error))
};
</script>

<template>
  <div>
    <div class="flex justify-end">
      <RouterLink to="listado-clientes"> Volver </RouterLink>
    </div>
    <Heading>{{ titulo }}</Heading>

    <div class="mx-auto mt-10 bg-white shadow">
      <div class="mx-auto md:w-2/3 py-20 px-6">
        <FormKit
          type="form"
          submit-label="Guardar cambios"
          incomplete-message="No se puedo envias, revisa los mensajes "
          @submit="handleSubmit"
          :value="formDta"
        >
          <FormKit
            type="text"
            label="Nombre"
            name="nombre"
            placeholder="Nombre de cliente"
            validation="required"
            :validation-messages="{
              required: 'EL nombre del cliente es Obligatorio',
            }"
            v-model="formDta.nombre"
          />
          <FormKit
            type="text"
            label="Apellido"
            name="apellido"
            placeholder="Apellido de cliente"
            validation="required"
            :validation-messages="{
              required: 'EL apellido del cliente es Obligatorio',
            }"
            v-model="formDta.apellido"
          />
          <FormKit
            type="email"
            label="Email"
            name="email"
            placeholder="Email de cliente"
            validation="required|email"
            :validation-messages="{
              required: 'EL email del cliente es Obligatorio',
              email: 'Coloca un email válido',
            }"
            v-model="formDta.email"
          />
          <FormKit
            type="text"
            label="Teléfono"
            name="telefono"
            placeholder="Teléfono:XXX-XXX-XXXX"
            validation="*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
            :validation-messages="{ matches: 'El formato no es válido' }"
            v-model="formDta.telefono"
          />
          <FormKit
            type="text"
            label="Empresa"
            name="empresa"
            placeholder="Empresa de cliente"
            v-model="formDta.empresa"
          />
          <FormKit
            type="text"
            label="Puesto"
            name="puesto"
            placeholder="Puesto de cliente"
            v-model="formDta.puesto"
          />
        </FormKit>
      </div>
    </div>
  </div>
</template>
<style >
.formkit-wrapper {
  max-width: 100%;
}
</style>
