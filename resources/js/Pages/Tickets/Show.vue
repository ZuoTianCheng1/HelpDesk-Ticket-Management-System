<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import TicketBadge from '@/Components/TicketBadge.vue'

const props = defineProps({ ticket: Object })
</script>

<template>
    <AuthenticatedLayout>
        <template #header>
            <h2 class="text-xl font-semibold text-gray-900">{{ ticket.title }}</h2>
        </template>

        <div class="py-8">
            <div class="mx-auto max-w-3xl px-4 sm:px-6 lg:px-8">
                <div class="rounded-xl bg-white p-6 shadow-sm">
                    <div class="flex flex-wrap gap-2">
                        <TicketBadge :label="ticket.priority" />
                        <TicketBadge :label="ticket.status" />
                        <span class="rounded-full bg-gray-100 px-2.5 py-1 text-xs font-semibold text-gray-700">
                            {{ ticket.category }}
                        </span>
                    </div>

                    <p class="mt-4 whitespace-pre-line text-gray-700">{{ ticket.description }}</p>

                    <div class="mt-6 flex gap-3">
                        <a
                            :href="route('tickets.edit', ticket.id)"
                            class="rounded-lg bg-indigo-600 px-4 py-2 text-sm font-semibold text-white"
                        >
                            Edit
                        </a>
                        <a
                            :href="route('tickets.index')"
                            class="rounded-lg border border-gray-300 px-4 py-2 text-sm font-semibold"
                        >
                            Back
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>