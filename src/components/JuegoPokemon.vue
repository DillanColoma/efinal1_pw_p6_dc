<template>
  <div class="image">
    <img :src="urlImg" alt="No se puede Mostra el pokemon">
    <label>{{texto}}</label>
  </div>
</template>

<script>
export default {
    props:{
        texto:{
            type: String,
        },
        urlImg:{
            type: String,
            
        }
    }   
}
</script>

<style>
.image{
  display: grid;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}
</style>
