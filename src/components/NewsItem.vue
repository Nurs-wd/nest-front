<template>
  <li>
    <h3 style="margin-right: 0px;">{{ el.title }}
    <button class="delete"
    v-on:click="$emit('deleteNews', el.id)"
    >&times;</button>
  </h3>
    <span>{{el.description}}</span> &nbsp;&nbsp;<br>
    <button @click="$emit('edit', el.id)"

    >Edit</button>
  </li>
</template>
<script>
export default {
  props: {
    el: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
  .delete{
    color: black;
    background-color: red;
    border-radius: 10px;
  }
  li{
    border: #2c3e50 solid 1px;
    margin: 5px 0;
    padding: 0 0 1rem 0;
  }
  button{
    margin-top: 10px;
  }
</style>