<script setup>
import { reactive, computed } from 'vue';
import {
  required,
  email,
  requiredPhone,
  minLength,
  maxLength,
  sameAs,
} from './utils/i18n-validators.js';
import { vMaska } from 'maska/vue';
import useVuelidate from '@vuelidate/core';

const form = reactive({
  name: null,
  email: null,
  phone: null,
  textarea: null,
  password: null,
  currentPassword: null,
  pending: null,
});

const rules = computed(() => ({
  name: {
    required,
  },
  email: {
    required,
    email,
  },
  phone: {
    required,
    requiredPhone: requiredPhone(18),
  },
  textarea: {
    required,
    minLength: minLength(5),
  },
  password: {
    required,
    minLength: minLength(5),
    maxLength: maxLength(15),
  },
  currentPassword: {
    required,
    minLength: minLength(5),
    maxLength: maxLength(15),
    sameAs: sameAs(form.password),
  },
  checkbox: {
    required,
    sameAs: sameAs(true),
  }
}));

const v = useVuelidate(rules, form);

const validate = ({ prop }) => {
  const error = v.value.$errors.find((el) => el.$property === prop);
  return error && error.$message;
};

const resetForm = () => {
  Object.keys(form).forEach((key) => { 
    form[key] = null;
  });
  v.value.$reset();
};

const onSubmit = () => {
  console.log(123);
};

const vMyMaska = {
  maska: vMaska,
};
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
    {{ form.phone }}
    <div class="field">
      <label class="label"> Phone Number </label>
      <div class="control">
        <input
          type="phone"
          v-model="form.phone"
          v-maska
          :data-maska="'+38 (###) - ### - ## - ##'"
          class="input"
        />
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
      <button
        @click="onSubmit"
        :class="{ 'is-loading': form.pending }"
        :disabled="form.pending"
        class="button is-link"
      >
        Send
      </button>
    </div>
  </div>
</template>
