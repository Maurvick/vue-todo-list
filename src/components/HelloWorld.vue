<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="increment">{{ count }}</button>
    <button @click="mutDeeply">{{ obj.count }} {{ obj.arr }}</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 1,
      obj: {
        nested: { count: 0 },
        arr: ["foo", "bar"],
      },
    };
  },
  methods: {
    increment() {
      this.count += this.count;
    },
    mutDeeply() {
      this.obj.nested.count++;
      this.obj.arr.push("baz");
    },
  },
  mounted() {
    console.log(this.count);
    this.count = 2;
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  text-decoration: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
