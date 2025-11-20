<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, provide, ref } from 'vue';
import ChevronDoubleRightIcon from '../icons/ChevronDoubleRightIcon.vue';
import EllispsisVertical from '../icons/EllispsisVertical.vue';
import Logo from '../icons/Logo.vue';
import Mark from '../icons/Mark.vue';

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

const name = ref('Royan Permana')
const email = ref('johndoe@example.com')

const initials = computed(() => {
    const parts = name.value.split(' ')
    let initials = ''
    if (parts.length > 0) {
        initials += parts[0].charAt(0)
    }
    if (parts.length > 1) {
        initials += parts[1].charAt(0)
    }
    return initials.toUpperCase()
})
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

            <div class="flex border-t border-gray-200 p-3">
                <img :src="`https://ui-avatars.com/api/?name=${initials}&background=c7d2fe&color=3730a3&bold=true`" alt=""
                    className="w-10 h-10 rounded-md" />
                <div class="flex items-center justify-between overflow-hidden transition-all"
                    :class="expanded ? 'ml-3 w-52' : 'w-0'">
                    <div class="leading-4">
                        <h4 class="font-semibold">{{ name }}</h4>
                        <span class="text-sm text-gray-600">{{ email }}</span>
                    </div>
                    <EllispsisVertical class="w-20" />
                </div>
            </div>
        </nav>
    </aside>
</template>
