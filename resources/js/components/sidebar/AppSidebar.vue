<script setup lang="ts">
import { onBeforeUnmount, onMounted, provide, ref } from 'vue';
import ChevronDoubleRightIcon from '../icons/ChevronDoubleRightIcon.vue';
import Logo from '../icons/Logo.vue';
import Mark from '../icons/Mark.vue';
import AppSidebarUser from './AppSidebarUser.vue';

const expanded = ref(false);

const setExpanded = () => {
    expanded.value = !expanded.value;
};

const handleResize = () => {
    if (window.innerWidth < 768) {
        expanded.value = false;
    } else {
        expanded.value = true;
    }
};

onMounted(() => {
    handleResize();
    window.addEventListener('resize', handleResize);
});

onBeforeUnmount(() => {
    window.removeEventListener('resize', handleResize);
});

provide('expanded', expanded)
</script>

<template>
    <aside class="h-screen w-max">
        <nav class="flex h-full flex-col border-r border-gray-200 bg-white shadow-sm">
            <div class="flex items-center p-4 pb-2" :class="expanded ? 'justify-between' : ''">
                <Logo class="overflow-hidden transition-all" :class="expanded ? 'w-32' : 'w-0'" />
                <button @click="setExpanded" class="rounded-lg bg-gray-50 py-1.5 hover:bg-gray-100 cursor-pointer">
                    <Mark v-if="expanded" />
                    <ChevronDoubleRightIcon v-else />
                </button>
            </div>

            <ul class="flex-1 px-3">
                <slot />
            </ul>

            <AppSidebarUser />
        </nav>
    </aside>
</template>
