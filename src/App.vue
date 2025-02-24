<script setup>
import { reactive } from 'vue';

const form = reactive({
  name: null,
  email: null,
  phone: null,
  textarea: null,
  password: null,
  currentPassword: null,
});
</script>

<template>
  <div class="container">
    <div class="field">
      <label class="label"> Name </label>
      <div class="control">
        <input type="text" v-model="form.name" class="input" />
      </div>
    </div>
    <div class="field">
      <label class="label"> Email </label>
      <div class="control">
        <input type="email" v-model="form.email" class="input" />
      </div>
    </div>
    <div class="field">
      <label class="label"> Phone Number </label>
      <div class="control">
        <input type="phone" v-model="form.phone" class="input" />
      </div>
    </div>
    <div class="field">
      <label class="label"> Message </label>
      <div class="control">
        <textarea type="text" v-model="form.textarea" class="textarea" />
      </div>
    </div>
    <div class="field">
      <label class="label"> Password </label>
      <div class="control">
        <input type="password" v-model="form.password" class="input" />
      </div>
    </div>
    <div class="field">
      <label class="label"> Current Password </label>
      <div class="control">
        <input type="password" v-model="form.currentPassword" class="input" />
      </div>
    </div>
    <div class="field">
      <div class="control">
        <label class="label">
          <input type="checkbox" v-model="form.checkbox" class="checkbox" />
          I'm agree with the <a href="#">community rules</a>
        </label>
      </div>
    </div>
    <div class="control mx-auto mt-2">
      <button class="button is-link" @click="onSubmit">Send</button>
    </div>
  </div>
</template>
