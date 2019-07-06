<template>
  <div id="app">
    <p>Total Like: {{ total }}</p>
    <like-component message="Like" @increment="incrementTotal"></like-component>
    <like-component message="Awesome" @increment="incrementTotal"></like-component>
    <like-component message="Great" @increment="incrementTotal"></like-component>
    <like-component></like-component>
  </div>
</template>

<script>
import Vue from "vue";
import { Stream } from "stream";

const likeComponent = Vue.extend({
  props: ["message"],
  props: {
    message: {
      type: String,
      default: "Like"
    }
  },
  data() {
    return {
      count: 0
    };
  },
  template: '<button @click="countUp">{{ message }} {{ count }}</button>',
  methods: {
    countUp() {
      this.count++;
      this.$emit("increment");
    }
  }
});

export default {
  components: {
    "like-component": likeComponent
  },
  data() {
    return { total: 0 };
  },
  methods: {
    incrementTotal() {
      this.total++;
    }
  }
};
</script>
