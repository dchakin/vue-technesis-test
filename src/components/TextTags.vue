<template>
  <div class="text-tags" :class="{ 'text-tags--center': alignment === 'center' }">
    <div v-for="(tag, index) in visibleTags" :key="index" class="text-tags__tag">
      <v-icon v-if="tag.icon">{{ tag.icon }}</v-icon>
      {{ tag.text }}
      <v-icon v-if="index < visibleTags.length - 1">mdi-circle-small</v-icon>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    tags: {
      type: Array,
      required: true,
    },
    alignment: {
      type: String,
      default: 'left',
      validator: (value) => ['left', 'center', 'width'].includes(value),
    },
  },
  computed: {
    visibleTags() {
      const maxWidth = this.alignment === 'width' ? this.calculateMaxWidth() : Infinity;
      let currentWidth = 0;
      let visibleTags = [];
      
      for (const tag of this.tags) {
        const tagWidth = this.calculateTagWidth(tag);
        if (currentWidth + tagWidth <= maxWidth) {
          visibleTags.push(tag);
          currentWidth += tagWidth;
        } else {
          break;
        }
      }
      
      return visibleTags;
    },
  },
  methods: {
    calculateMaxWidth() {
      // Calculate the maximum width based on the parent container's width
      const parentWidth = this.$el.parentElement.clientWidth;
      // You can apply any logic here based on your design requirements
      return parentWidth;
    },
    calculateTagWidth(tag) {
      // Calculate the width of a tag based on its content and icon (if present)
      // You can apply any logic here based on your design requirements
      return tag.text.length * 10 + (tag.icon ? 20 : 0);
    },
  },
};
</script>

<style scoped lang="scss">
.text-tags {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  
  &--center {
    justify-content: center;
  }
  
  &__tag {
    display: flex;
    align-items: center;
    margin-right: 8px;
    margin-bottom: 8px;
  }
}
</style>
