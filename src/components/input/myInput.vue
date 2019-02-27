<template>
    <input
        class="content-input-city"
        type="text"
        :placeholder="placeholder"
      >
</template>

<script>
export default {
    name: 'myInput',
    props: {
      placeholder: {
        type: String,
        default:'请输入...'
      }
    },
    data() {
      return {

      }
    }
}
</script>

<style scpoed>
.content-input-city {
    width: 100%;
    height: 35px;
    line-height: 35px;
    border-radius: 10px;
    display: inline-block;
    outline: none;
    font-size: 1rem;
    box-shadow: none;
    border: none;
    background: ghostwhite;
    padding:0 15px;
    transition: width 0.2s ease-in-out;
    box-sizing: border-box;
    color: #34495e;
}
.content-input-city::-webkit-input-placeholder {
    color: #bdc3c7;
    font-size: 1rem;
    text-align: left;
}
.content-input-city:focus {
    width: 85%;
}
</style>
