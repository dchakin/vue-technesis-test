<template>
  <div class="main-container">
    <text-tags :tags="tags" alignment="left" :style="{background: '#FED6BC'}"/>
    <text-tags :tags="tags" alignment="width" :tags-font-size="20" :style="{ background: '#FFFADD'}"/>
    <text-tags :tags="tags" alignment="left" :tags-font-size="24" :style="{width: '50%', background: '#DEF7FE'}" />
    <text-tags :tags="tags" alignment="width" :tags-font-size="32" :style="{width: '50%', background: '#E7ECFF'}"/>
  </div>
</template>

<script>
import TextTags from "@/components/TextTags.vue";

export default {
  data() {
    return {
      tags: [
        { text: "test text 1" },
        { text: "test text 2", icon: "mdi-home" },
        { text: "test 3", icon: "mdi-account-heart-outline" },
        { text: "testtext 4", icon: "mdi-airplane" },
        { text: "testt 5", icon: "mdi-apps" },
        { text: "t", icon: "mdi-axis" },
      ],
    };
  },
  components: {
    TextTags,
  },
};
</script>

<style scoped lang="scss">
.main-container {
  width: 100%;
  font-size: 20px;
}

</style>
