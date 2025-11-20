<script setup lang="ts">
import { computed, inject, ref, nextTick, watch, onBeforeUnmount } from 'vue';
import EllispsisVertical from '../icons/EllispsisVertical.vue';

const expanded = inject('expanded')

const name = ref('Royan Permana')
const email = ref('johndoe@example.com')

const initials = computed(() => {
    const parts = name.value.split(' ')
    let initialsValue = ''
    if (parts.length > 0) {
        initialsValue += parts[0].charAt(0)
    }
    if (parts.length > 1) {
        initialsValue += parts[1].charAt(0)
    }
    return initialsValue.toUpperCase()
})

const isOpen = ref(false);
const buttonRef = ref<HTMLButtonElement | null>(null); // Ref for the ellipsis button
const imageRef = ref<HTMLImageElement | null>(null); // New ref for the avatar image
const menuRef = ref<HTMLDivElement | null>(null);

const toggleMenu = async () => {
    isOpen.value = !isOpen.value;
    if (isOpen.value) {
        await nextTick();
        positionMenu();
    }
};

const positionMenu = () => {
    // Determine which element triggered the menu based on the expanded state
    const triggerElement = expanded.value ? buttonRef.value : imageRef.value;

    if (!triggerElement || !menuRef.value) return;

    const rect = triggerElement.getBoundingClientRect(); // Position of the current trigger element
    const menu = menuRef.value;
    const margin = 8; // 8px margin

    const menuWidth = menu.offsetWidth;
    const menuHeight = menu.offsetHeight;
    const windowWidth = window.innerWidth;
    const windowHeight = window.innerHeight;

    let menuTop = rect.top - menuHeight; // Target: menu's bottom aligns with button's top
    let menuLeft = rect.right + margin;

    // Ensure it doesn't go off the top edge, add a small margin
    if (menuTop < margin) {
        menuTop = margin; // Set to margin from top
    }

    // Adjust vertical position if it overflows the bottom
    if (menuTop + menuHeight + margin > windowHeight) {
        menuTop = windowHeight - menuHeight - margin;
    }

    // Adjust horizontal position if it overflows the right
    if (menuLeft + menuWidth + margin > windowWidth) {
        // Position it to the left of the button instead
        menuLeft = rect.left - menuWidth;
    }

    menu.style.top = `${menuTop}px`;
    menu.style.left = `${menuLeft}px`;
};

const closeOnClickOutside = (event: MouseEvent) => {
    const triggerElement = expanded.value ? buttonRef.value : imageRef.value;

    if (
        isOpen.value &&
        menuRef.value &&
        !menuRef.value.contains(event.target as Node) &&
        triggerElement &&
        !triggerElement.contains(event.target as Node)
    ) {
        isOpen.value = false;
    }
};

watch(isOpen, (value) => {
    if (value) {
        document.addEventListener('click', closeOnClickOutside);
    } else {
        document.removeEventListener('click', closeOnClickOutside);
    }
});

onBeforeUnmount(() => {
    document.removeEventListener('click', closeOnClickOutside);
});

</script>

<template>
    <div class="flex border-t border-gray-200 p-3">
        <img ref="imageRef" @click="toggleMenu" :src="`https://ui-avatars.com/api/?name=${initials}&background=c7d2fe&color=3730a3&bold=true`" alt=""
            className="w-10 h-10 rounded-md cursor-pointer" />
        <div class="flex items-center justify-between overflow-hidden transition-all"
            :class="expanded ? 'ml-3 w-52' : 'w-0'">
            <div class="leading-4">
                <h4 class="font-semibold">{{ name }}</h4>
                <span class="text-sm text-gray-600">{{ email }}</span>
            </div>
            <div class="relative">
                <button ref="buttonRef" @click="toggleMenu"
                    class="rounded-lg bg-gray-50 p-1.5 hover:bg-gray-100 cursor-pointer">
                    <EllispsisVertical class="w-6" />
                </button>

                <div v-if="isOpen" ref="menuRef"
                    class="fixed w-48 bg-white border border-gray-200 rounded-md shadow-lg z-50">
                    <ul class="py-1">
                        <li>
                            <a href="#" class="block px-4 py-2 hover:bg-gray-100">Profil</a>
                        </li>
                        <li>
                            <a href="#" class="block px-4 py-2 hover:bg-gray-100">Pengaturan</a>
                        </li>
                        <li>
                            <button class="w-full text-left px-4 py-2 hover:bg-gray-100">
                                Logout
                            </button>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>
