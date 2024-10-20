<script setup>
import { ref } from 'vue'
import ListItem from './components/ListItem.vue';
import { invoke } from '@tauri-apps/api/tauri'

const items = ref([])
const hoveredItemIndex = ref(null)
const selectedItemIndex = ref(null)
const itemsRef = ref([])

const handleKeyPress = (e) => {
  if (e.keyCode === 38) {
    e.preventDefault();
    if (selectedItemIndex.value > 0) {
      selectedItemIndex.value -= 1;
    }
    itemsRef.value[selectedItemIndex.value].$el.scrollIntoView({ behavior: 'smooth', block: 'nearest' })
  }
  if (e.keyCode === 40) {
    e.preventDefault();
    if (selectedItemIndex.value < items.value.length - 1) {
      selectedItemIndex.value += 1;
    }
    itemsRef.value[selectedItemIndex.value].$el.scrollIntoView({ behavior: 'smooth', block: 'nearest' })
  }
  if (e.keyCode === 13) {
    invoke("set_clipboard", { text: items.value[selectedItemIndex.value]['text'] })
  }
};
window.addEventListener('keydown', handleKeyPress);

invoke("get_items").then(fetched_items => {
  items.value = fetched_items
})

</script>

<template>
  <div class="bg-neutral-900 flex flex-col h-screen accent-slate-950">
    <div class="bg-neutral-800 p-2">
      <input placeholder="Search..."
             class="focus:outline outline-slate-500 outline-1 w-full p-2 rounded-md bg-neutral-800 text-white border border-neutral-900">
    </div>
    <ul class="bg-neutral-800 flex-grow overflow-y-scroll scrollbar-hide focus-within:scrollbar-default">
      <ListItem v-for="
        (item,
          index)
        in
            items"
                ref="itemsRef"
                :item="item"
                :highlighted="index === selectedItemIndex"
                :hovered="index === hoveredItemIndex"
                :index="index"
                @mouseover="hoveredItemIndex = index"
                @mouseout="hoveredItemIndex = null"
                @delete="(i) => {
                  if (i === selectedItemIndex) {
                    selectedItemIndex = null;
                  } else if (i < selectedItemIndex) {
                    selectedItemIndex--;
                  }
                  items.splice(i, 1)
                }"
                @click="selectedItemIndex = index" />
    </ul>
  </div>
</template>

<style scoped></style>
