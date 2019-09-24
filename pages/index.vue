<template>
  <v-flex>
    <v-container fluid grid-list-lg class="mt-5">
      <posts />
    </v-container>
  </v-flex>
</template>

<script>
import posts from '~/components/Posts.vue'

export default {
  components: { posts }
}
</script>
