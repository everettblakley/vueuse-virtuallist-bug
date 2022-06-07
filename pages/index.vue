<script setup>
import { useToggle, useVirtualList } from '@vueuse/core'
import { computed } from 'vue'

const [isEven, toggle] = useToggle()
const allItems = Array.from(Array(99999).keys())
const filteredList = computed(() => allItems.filter(i => isEven.value ? i % 2 === 0 : i % 2 === 1))

const { list, containerProps, wrapperProps } = useVirtualList(
  filteredList,
  {
    itemHeight: 22,
  },
)

/**
 * Vue 2 requires the container ref to be bound explicitly
 * @see https://github.com/vueuse/vueuse/issues/1364
 */
const containerRef = containerProps.ref
</script>

<template>
  <div class="w-full-h-screen">
    <p>Showing {{ isEven ? 'even' : 'odd' }} items</p>
    <button class="btn" @click="toggle">
      Toggle Even/Odd
    </button>
    <div v-bind="containerProps" ref="containerRef" style="height: 300px">
      <div v-bind="wrapperProps">
        <div v-for="item in list" :key="item.index" style="height: 22px">
          Row: {{ item.data }}
        </div>
      </div>
    </div>
  </div>
</template>
