<template>
    <AppLayout title="Bank Accounts">
        <Table
            :headers="getHeadersTable('bankAccountHeader')"
            :text-empty="bankAccounts.data.length > 0 ? '' : 'There are no recent bank accounts.'"
            card-title="Bank Accounts"
        >
            <template #body>
                <tr
                    v-for="(bankAccount, index) of bankAccounts.data"
                    :key="index"
                    :style="[setHeightForTable()]"
                >
                    <td>{{ bankAccount.created }}</td>
                    <td>
                        <span
                            :data-bs-target="`#${target}`"
                            data-bs-toggle="modal"
                            class="custom__link"
                            @click="chooseBankAccount(bankAccount)"
                        >
                            {{ bankAccount.bankId }}
                        </span>
                    </td>
                    <td>
                        <Link
                            v-if="bankAccount.user"
                            :href="route('....')"
                            class="custom__link"
                        >
                            {{ bankAccount?.user?.fullName }} ({{ bankAccount?.user?.email }})
                        </Link>
                    </td>
                    <td>{{ bankAccount.bankName }}</td>
                    <td>{{ bankAccount.typeAccountLabel }}</td>
                    <td><span :class="bankAccount.badgeClass">{{ bankAccount.status }}</span></td>
                    <td>
                        <ActionsTD>
                            <template #custom-action>
                                <li
                                    v-for="(action, index) in actions"
                                    :key="index"
                                >
                                    <button
                                        v-if="action.value !== bankAccount.systemStatus"
                                        type="button"
                                        class="dropdown-item"
                                        @click="updateBankAccount(bankAccount, action.value)"
                                    >
                                        <i :class="action.icon" />
                                        {{ action.name }}
                                    </button>
                                </li>
                                <li>
                                    <button
                                        :data-bs-target="`#${target}`"
                                        type="button"
                                        class="dropdown-item"
                                        data-bs-toggle="modal"
                                        @click="chooseBankAccount(bankAccount)"
                                    >
                                        <i class="ri-eye-line me-2 align-middle"></i>
                                        More Info
                                    </button>
                                </li>
                            </template>
                        </ActionsTD>
                    </td>
                </tr>
            </template>

            <template #footer>
                <Pagination
                    v-if="bankAccounts.data.length > 0"
                    :current-page="bankAccounts.meta.current_page"
                    :per-page="bankAccounts.meta.per_page"
                    :total-items="bankAccounts.meta.total"
                    :last-page="bankAccounts.meta.last_page"
                    @pagination-page-change="(page) => getBankAccounts(page)"
                />
            </template>
        </Table>
    </AppLayout>

    <InfoBankAccountModal
        :bank-account="bankAccount"
        :target="target"
    />
</template>

<script setup>
import { ref } from "vue";
import { router, Link } from "@inertiajs/vue3";
import { getHeadersTable } from "@/scripts/utils/functions";
import AppLayout from '~/Layouts/AppLayout.vue';
import Table from '~/Components/Tables/TableGrid.vue';
import Pagination from '~/Components/Pagination/Pagination.vue';
import ActionsTD from "~/Components/Tables/ActionsTD.vue";
import InfoBankAccountModal from "~/Components/Modals/BankAccount/InfoBankAccountModal.vue";


const props = defineProps({
    bankAccounts: {
        type: Array,
        required: true
    },
    actions: {
        type: Array,
        required: true
    },
});

const target = 'infoBankAccount';
const bankAccount = ref(null);


const getBankAccounts = (page) => {
    router.visit(window.route('.....', {
        page,
    }), {
        preserveScroll: true,
        preserveState: true,
    });
};

const chooseBankAccount = (chooseBankAccount) => {
    bankAccount.value = chooseBankAccount;
}

const updateBankAccount = (chooseBankAccount, status) => {
    router.put(window.route('.....'), {
        status: status
    });
}

const setHeightForTable = () => {
    const length = props.bankAccounts.data.length;

    switch (length) {
        case 1:
            return 'height: 190px';
        case 2:
            return 'height: 125px';
        default:
            return '';
    }
}
</script>

<style scoped>

</style>
