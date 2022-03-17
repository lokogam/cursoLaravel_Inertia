<template>
  <app-layout>
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">User</h2>
    </template>

    <div class="py-12">
      <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
          <div class="p-6 sm:px-20 bg-white border-b border-gray-200">
            <form @submit.prevent="submit">
              <div class="mt-4">
                <jet-label value="Nombre:" />
                <jet-input-error :message="errors.name" />
                <jet-input
                  class="mt-1 block w-full"
                  type="text"
                  v-model="form.name"
                />
              </div>
              <div class="mt-4">
                <jet-label value="Email:" />
                <jet-input-error :message="errors.email" />
                <jet-input
                  class="mt-1 block w-full"
                  type="text"
                  v-model="form.email"
                />
              </div>
              <div class="mt-4">
                <jet-label value="Contraseña:" />
                <jet-input-error :message="errors.password" />
                <jet-input
                  class="mt-1 block w-full"
                  type="password"
                  v-model="form.password"
                />
              </div>
              <div class="mt-4"
                v-if="!customer.profile_photo_path"
              >
                <jet-label value="Avatar:" />
                <jet-input-error :message="errors.avatar" />
                <jet-input
                  
                  class="mt-1 block w-full"
                  type="file"
                  
                  @input="form.avatar = $event.target.files[0]"

                />
              </div>
              <jet-button class="mt-4" type="submit">Enviar</jet-button>


              <div v-if="customer.profile_photo_url">

                <hr class="my-4">

                <img class="h-20 w-20 rounded-full border object-cover m-auto block" :src="customer.profile_photo_url" :alt="customer.name">

                <inertia-link v-if="customer.profile_photo_path" class="block text-center text-red-600 text-xs font-bold m-0" href="#" @click="deleteAvatar">Eliminar</inertia-link>
              </div>

            </form>
          </div>
        </div>
      </div>
    </div>
  </app-layout>
</template>

<script>
import { Inertia } from "@inertiajs/inertia";

import AppLayout from "@/Layouts/AppLayout";

import JetInputError from "@/Jetstream/InputError";
import JetInput from "@/Jetstream/Input";
import JetLabel from "@/Jetstream/Label";
import JetButton from "@/Jetstream/Button";

export default {
  props: ["customer", "errors"],
  data() {
    return {
      form: {
        name: this.customer.name,
        email: this.customer.email,
        avatar: "",
        password: "",
      },
    };
  },
  components: {
    AppLayout,
    JetInputError,
    JetInput,
    JetLabel,
    JetButton
  },
  methods: {
    deleteAvatar: function(){
      Inertia.delete(route('user.deleteAvatar', {customer: this.customer}))
    },
    submit() {
      //alert(this.form.avatar)
      //Inertia.put(route("user.update",{'customer':this.customer}), this.form);
      Inertia.post(route("user.update",{'customer':this.customer}),
      {
        _method:'put',
        name: this.form.name,
        email: this.form.email,
        password: this.form.password,     
        avatar: this.form.avatar
      });
    },
  },
};
</script>
