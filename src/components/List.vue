<script setup>
defineProps(['name', 'seller', 'picked'])
defineEmits(['delete', 'edit'])
</script>

<template>
  <div >
    <button @click="$emit('edit')">Редактировать</button>
    <button @click="$emit('delete')">Удалить</button>
    <div>название товара - {{ name }}</div>
    <div>селлер - {{ seller }}</div>
    <div>способ интеграции - {{ picked }}</div>
    <hr>
  </div>
</template>
