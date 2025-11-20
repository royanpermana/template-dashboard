<script setup lang="ts">
import ArchiveBox from '@/components/icons/ArchiveBox.vue';
import ChartBar from '@/components/icons/ChartBar.vue';
import Cog6Tooth from '@/components/icons/Cog6Tooth.vue';
import InformationCircle from '@/components/icons/InformationCircle.vue';
import ReceiptPercent from '@/components/icons/ReceiptPercent.vue';
import Squares from '@/components/icons/Squares.vue';
import Truck from '@/components/icons/Truck.vue';
import UserCircle from '@/components/icons/UserCircle.vue';
import AppSidebar from '@/components/sidebar/AppSidebar.vue';
import AppSidebarItem from '@/components/sidebar/AppSidebarItem.vue';

</script>

<template>
    <div class="flex">
        <AppSidebar >
            <AppSidebarItem :icon="Squares" text="Dashboard" alert />
            <AppSidebarItem :icon="ChartBar" text="Statistic" active />
            <AppSidebarItem :icon="UserCircle" text="Users" />
            <AppSidebarItem :icon="ArchiveBox" text="Inventory" />
            <AppSidebarItem :icon="Truck" text="Orders" alert />
            <AppSidebarItem :icon="ReceiptPercent" text="Billings" />
            <hr class="my-3 text-gray-200">
            <AppSidebarItem :icon="Cog6Tooth" text="Settings" />
            <AppSidebarItem :icon="InformationCircle" text="Help" />
        </AppSidebar>

        <div class="p-2">
            <slot />
        </div>
    </div>
</template>
