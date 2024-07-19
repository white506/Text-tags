<template>
  <div :class="['text-tags', alignmentClass]" ref="container">
    <div v-for="(tag, index) in visibleTags" :key="'tag-' + index" class="text-tags__item">
      <v-icon v-if="tag.icon">{{ tag.icon }}</v-icon>
      <span class="text-tags__text">{{ tag.text }}</span>
      <v-icon v-if="index < visibleTags.length - 1">mdi-circle-small</v-icon>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TextTags',
  props: {
    tags: {
      type: Array,
      required: true
    },
    alignment: {
      type: String,
      default: 'left',
      validator: value => ['left', 'justified'].includes(value)
    }
  },
  data() {
    return {
      visibleTags: []
    };
  },
  computed: {
    alignmentClass() {
      return this.alignment === 'justified' ? 'text-tags--justified' : 'text-tags--left';
    }
  },
  mounted() {
    this.updateVisibleTags();
    window.addEventListener('resize', this.updateVisibleTags);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.updateVisibleTags);
  },
  methods: {
    updateVisibleTags() {
      this.$nextTick(() => {
        const containerWidth = this.$refs.container.clientWidth;
        let totalWidth = 0;
        this.visibleTags = [];

        for (let tag of this.tags) {
          const tagWidth = this.getTagWidth(tag);
          if (totalWidth + tagWidth <= containerWidth) {
            this.visibleTags.push(tag);
            totalWidth += tagWidth;
          } else {
            break;
          }
        }
      });
    },
    getTagWidth(tag) {
      const tagElement = document.createElement('span');
      tagElement.style.visibility = 'hidden';
      tagElement.style.position = 'absolute';
      tagElement.style.whiteSpace = 'nowrap';
      tagElement.innerText = tag.text;
      document.body.appendChild(tagElement);
      const width = tagElement.offsetWidth;
      document.body.removeChild(tagElement);
      return width + 24; // Include padding and separator icon width
    }
  }
};
</script>

<style lang="scss">
.text-tags {
  display: flex;
  align-items: center;
  flex-wrap: nowrap;
  overflow: hidden;

  &__item {
    display: flex;
    align-items: center;
    margin: 0 4px;
  }

  &__text {
    margin: 0 4px;
  }

  &--left {
    justify-content: flex-start;
  }

  &--justified {
    justify-content: space-between;
  }
}
</style>
