<script setup lang="ts">
import { inject } from 'vue';

defineProps<{
    icon: any,
    text: string,
    active?: boolean,
    alert?: boolean
}>()

const expanded = inject('expanded')
</script>

<template>
    <li class="relative my-1 flex cursor-pointer items-center rounded-md px-1 py-2 font-medium transition-colors group"
        :class="active ? 'bg-gradient-to-tr from-indigo-200 to-indigo-100 text-indigo-800' : 'text-gray-600 hover:bg-indigo-50'">
        <component :is="icon" />
        <span class="overflow-hidden transition-all" :class="expanded ? 'w-52 ml-3' : 'w-0'">{{ text }}</span>
        <div v-if="alert" class="absolute right-2 h-2 w-2 rounded bg-indigo-400" :class="expanded ? '' : 'top-2'" />

        <div v-if="!expanded" class="absolute left-full ml-6 rounded-md bg-indigo-100 px-2 py-1 text-sm text-indigo-800 opacity-20 invisible
            -translate-x-3 transition-all group-hover:visible group-hover:opacity-100 group-hover:translate-x-0">
            {{ text }}
        </div>
    </li>
</template>
