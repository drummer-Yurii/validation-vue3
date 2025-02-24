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
    requiredPhone: requiredPhone(24),
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
  },
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

const onSubmit = async () => {
  v.value.$touch();
  if (form.pending) return;
  if (await v.value.$validate()) {
    form.pending = true;
    try {
      const payload = {
        name: form.name,
        email: form.email,
        phone: form.phone,
        textarea: form.textarea,
        password: form.password,
        currentPassword: form.currentPassword,
        checkbox: form.checkbox,
      };
      setTimeout(() => {
        console.log(payload);
        console.log('Send Request');
        resetForm();
      }, 2500);
    } catch (e) {
      console.log(e);
    }
  }
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
        <input
          type="text"
          v-model="form.name"
          @focus="v.$reset()"
          :class="{ 'is-danger': !!validate({ prop: 'name' }) }"
          class="input"
        />
        <p v-if="!!validate({ prop: 'name' })" class="help is-danger">
          {{ validate({ prop: 'name' }) }}
        </p>
      </div>
    </div>
    <div class="field">
      <label class="label"> Email </label>
      <div class="control">
        <input
          type="email"
          v-model="form.email"
          @focus="v.$reset()"
          class="input"
          :class="{ 'is-danger': !!validate({ prop: 'email' }) }"
        />
        <p v-if="!!validate({ prop: 'email' })" class="help is-danger">
          {{ validate({ prop: 'email' }) }}
        </p>
      </div>
    </div>
    <div class="field">
      <label class="label"> Phone Number </label>
      <div class="control">
        <input
          type="phone"
          v-model="form.phone"
          @focus="v.$reset()"
          v-maska
          :data-maska="'+38 (###) - ### - ## - ##'"
          class="input"
          :class="{ 'is-danger': !!validate({ prop: 'phone' }) }"
        />
        <p v-if="!!validate({ prop: 'phone' })" class="help is-danger">
          {{ validate({ prop: 'phone' }) }}
        </p>
      </div>
    </div>
    <div class="field">
      <label class="label"> Message </label>
      <div class="control">
        <textarea
          type="text"
          v-model="form.textarea"
          @focus="v.$reset()"
          class="textarea"
          :class="{ 'is-danger': !!validate({ prop: 'textarea' }) }"
        />
        <p v-if="!!validate({ prop: 'textarea' })" class="help is-danger">
          {{ validate({ prop: 'textarea' }) }}
        </p>
      </div>
    </div>
    <div class="field">
      <label class="label"> Password </label>
      <div class="control">
        <input
          type="password"
          v-model="form.password"
          @focus="v.$reset()"
          class="input"
          :class="{ 'is-danger': !!validate({ prop: 'password' }) }"
        />
        <p v-if="!!validate({ prop: 'password' })" class="help is-danger">
          {{ validate({ prop: 'password' }) }}
        </p>
      </div>
    </div>
    <div class="field">
      <label class="label"> Current Password </label>
      <div class="control">
        <input
          type="password"
          v-model="form.currentPassword"
          @focus="v.$reset()"
          class="input"
          :class="{ 'is-danger': !!validate({ prop: 'currentPassword' }) }"
        />
        <p v-if="!!validate({ prop: 'currentPassword' })" class="help is-danger">
          {{ validate({ prop: 'currentPassword' }) }}
        </p>
      </div>
    </div>
    <div class="field">
      <div class="control">
        <label class="label" :class="{ 'has-text-danger': !!validate({ prop: 'checkbox' }) }">
          <input type="checkbox" v-model="form.checkbox" class="checkbox" @focus="v.$reset()" />
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
