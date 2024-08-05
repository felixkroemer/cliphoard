<template>
    <li class="p-2 flex place-items-center"
        :class="{ 'bg-slate-800': highlighted && !hovered, 'bg-slate-700': hovered }">
        <p class="text-white flex-1 line-clamp-2 mr-2">
            {{ item.text }}
        </p>
        <button type="button"
                @click.stop="$emit('delete', index)"
                class="bg-white rounded-full p-2 text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none">
            <div>
                <svg class="h-3 w-3"
                     xmlns="http://www.w3.org/2000/svg"
                     fill="none"
                     viewBox="0 0 24 24"
                     stroke="currentColor"
                     aria-hidden="true">
                    <path stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="3"
                          d="M20 4L4 20M4 4L20 20" />
                </svg>
            </div>
        </button>
    </li>
</template>

<script setup>
defineProps(['item', 'highlighted', 'hovered', 'index']);
</script>

<style scoped></style>
