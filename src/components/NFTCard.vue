<script>
export default {
  props: ['title', 'description', 'image']
}
</script>

<template>
    <h3>{{ title }}</h3>
    <p>{{ description }}</p>
    <img :src="image" />
</template>
