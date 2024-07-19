<template>
  <v-app>
    <v-container>
      <text-tags
        :tags="tags"
        alignment="left"
        class="full-width"
      />
      <text-tags
        :tags="tags"
        alignment="justified"
        class="full-width"
      />
      <text-tags
        :tags="tags"
        alignment="left"
        class="half-width"
      />
      <text-tags
        :tags="tags"
        alignment="justified"
        class="half-width"
      />
    </v-container>
  </v-app>
</template>

<script>
import TextTags from './components/TextTags.vue';

export default {
  name: 'App',
  components: {
    TextTags
  },
  data() {
    return {
      tags: [
        { text: '4,3', icon: 'mdi-star' },
        { text: 'Фридланские ворота' },
        { text: '1,2 км от вас' }
      ]
    };
  }
};
</script>

<style>
.full-width {
  width: 100%;
  margin-bottom: 16px;
}

.half-width {
  width: 50%;
  margin-bottom: 16px;
}
</style>
