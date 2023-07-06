<template>
    <AppLayout title="Conversions">
        <Table
            :headers="getHeadersTable('conversionHeader')"
            :text-empty="conversions.data.length > 0 ? '' : 'There are no recent conversions.'"
            card-title="Conversions"
            @change-date-type="viewDateTypeAgo = !viewDateTypeAgo"
        >
            <template #body>
                <tr v-for="(conversion, index) of conversions.data" :key="index">
                    <td v-if="!viewDateTypeAgo">{{ conversion.created }}</td>
                    <td v-else>{{ conversion.createdAgo }}</td>
                    <td>{{ conversion.conversionId }}</td>
                    <td>{{ conversion.orderId }}</td>
                    <td>
                        <span :class="conversion.badgeClass">{{ conversion.status }}</span>
                    </td>
                    <td>
                        <div class="d-flex w-50 flex-row justify-content-between">
                            <div>{{ conversion.toAmount }}</div>
                            <div><Icon :name="`svg-crypto-icons-${conversion.toCoin.toLowerCase()}`" width="18" height="18" /></div>
                        </div>
                    </td>
                    <td>
                        <div class="d-flex w-50 flex-row justify-content-between">
                            <div>{{ conversion.fromAmount }}</div>
                            <div><Icon :name="`svg-crypto-icons-${conversion.fromCoin.toLowerCase()}`" width="18" height="18" /></div>
                        </div>
                    </td>
                </tr>
            </template>
        </Table>
    </AppLayout>
</template>

<script setup>
import { defineProps, ref } from 'vue';
import { getHeadersTable } from "@/scripts/utils/functions";
import { Link } from "@inertiajs/vue3";
import AppLayout from "~/Layouts/AppLayout.vue";
import Table from '~/Components/Tables/TableGrid.vue';
import Icon from "~/Components/UI/Icon.vue";


const props = defineProps({
    conversions: {
        type: Array,
        required: true
    }
});

const viewDateTypeAgo = ref(false);
</script>

<style scoped>

</style>