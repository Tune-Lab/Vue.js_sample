<template>
  <AuthLayout title="Create">
    <CircleButton
      :url="route('.....')"
      class="circle__button"
      icon="arrow-left"
    />
    <h1>Create a new workspace</h1>

    <form
      class="flex flex-col items-center mt-12 space-y-10"
      @submit.prevent="createWorkspace"
    >
      <div class="flex flex-col space-y-5">
        <Input
          v-model="form.name"
          :error="form.errors.name"
          is-required
          label="Name"
          placeholder="Enter workspace name"
        />
        <Input
          v-model="form.domain"
          :error="form.errors.domain"
          is-required
          label="Subdomain"
          placeholder="Enter workspace subdomain. Ex: local"
        />
      </div>

      <Button
        label="Create workspace"
        @click="createWorkspace"
      />
    </form>
  </AuthLayout>
</template>

<script setup>
import { useForm } from '@inertiajs/inertia-vue3';
import AuthLayout from '~/Layouts/AuthLayout.vue';
import Input from '~/Shared/Form/Input.vue';
import Button from '~/Shared/Form/Button.vue';
import CircleButton from '~/Shared/Form/CircleButton.vue';

const form = useForm({
  name: null,
  domain: null,
});

const createWorkspace = () => form.post(window.route('.....'));
</script>
