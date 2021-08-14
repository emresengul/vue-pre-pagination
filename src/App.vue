<template>
    <pagination :records="50" :start="25"></pagination>
</template>

<script>
    import Pagination from "./components/Pagination.vue";
    export default {
      components: { Pagination },
      name: "App",
    };
</script>

<style>
.custom-pagination-class {
    width: 42px;
    height: 42px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: blue;
    margin-right: 0.45rem;
    color: #fff;
}
.custom-active-class {
    background-color: gray;
    color: #000;
    border-radius: 25px;
}
</style>
