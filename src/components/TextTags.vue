<template>
  <div ref="refRow" class="text-tags" :class="{ 'text-tags--width': alignment === 'width' }">
    <div v-for="(tag, index) in visibleTags" :key="index" class="text-tags__row" :style="{'fontSize': `${tagsFontSize}px`}">
      <v-icon
        v-if="tag === 'divider'"
        class="text-tags__divider"
        :style="{'font-size': `${iconSize}px`}"
        :class="{'display-none': index === visibleTags.length - 1}"
      >
        mdi-circle-small
      </v-icon>
      
      <div v-else class="text-tags__tag">
        <v-icon color="#fff" :style="{'font-size': `${iconSize}px`}" v-if="tag.icon">{{ tag.icon }}</v-icon>
        {{ tag.text }}
      </div>
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
      validator: (value) => ['left', 'width'].includes(value),
    },
    tagsFontSize: {
      type: Number,
      required: false,
      default: 16,
    }
  },
  
  data() {
    return {
      maxWidth: 0, // максимальная ширина компонента
      iconSize: 20, // размер иконки
    }
  },
  
  computed: {
    // отображаемые теги
    visibleTags() {
      let totalWidth = 0 // общая ширина всех тегов
      let visibleTags = [] // массив отображаемых тегов
      
      // проходимся по списку тегов
      for (const tag of this.tags) {
        // для кажого тега будем считать его ширину
        const tagWidth = this.calculateTagWidth(tag)
        
        // если общая ширина всех тегов меньше максимальной ширины компонента, то отображаем тег
        if (totalWidth + tagWidth < this.maxWidth) {
          visibleTags.push(tag) // пушим тег в массив
          visibleTags.push('divider') // пушим разделитель
          totalWidth += tagWidth // увеличиваем общую сумму ширины всех тегов
          if (visibleTags.length !== 1) {
            totalWidth += this.iconSize // увеличиваем общую сумму ширины всех тегов на ширину разделителей
          }
        } else {
          break
        }
      }
      
      visibleTags.pop() // удялем последний разделитель
      return visibleTags
    },
  },
  
  mounted() {
    // высчитываем максимальную ширину компонента при монтировании
    this.calculateMaxWidth()
    // высчитываем максимальную ширину компонента при ресайзе окна
    window.addEventListener('resize', () => this.calculateMaxWidth())
  },
  
  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
  },
  
  methods: {
    // метод вычисления максимальной ширины компонента
    calculateMaxWidth() {
      this.maxWidth = this.$el.clientWidth // вычисляется по ширине корневого элемента в компоненте
    },
    
    // метод вычисления ширины тега
    calculateTagWidth(tag) {
      const tempElement = document.createElement('div') // создаем временный элемент нашего тега
      tempElement.style.visibility = 'hidden' // скрываем
      tempElement.style.position = 'absolute' // позиционриуем абсолютно
      tempElement.className = 'text-tags__tag' // применяем стили
      tempElement.style.fontSize = `${this.tagsFontSize}px` // скрываем
      tempElement.innerHTML = tag.text // заполняем текст
      
      this.$el?.appendChild(tempElement) // добавляем тег на страницу
      const tagWidth = tempElement.clientWidth // фиксируем ширину тега
      this.$el?.removeChild(tempElement) // удаляем временный тег
      
      return tagWidth + (tag.icon ? this.iconSize + 4 : 0) // this.iconSize + 4px (column-gap)
    },
  },
};
</script>

<style lang="scss">
.text-tags {
  display: flex;
  align-items: center;
  padding: 24px 0;
  
  &__row {
    display: flex;
    align-items: center;
  }
  
  &--width {
    justify-content: space-between;
  }
  
  &--left {
    justify-content: flex-start;
  }
  
  &__tag {
    display: flex;
    align-items: center;
    column-gap: 4px;
    background: #8990b3;
    color: #fff;
    border-radius: 16px;
    padding: 0 8px;
    min-height: 24px;
    white-space: nowrap;
  }
}
</style>
