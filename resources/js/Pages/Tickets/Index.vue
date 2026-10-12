<script setup>
import { useForm } from '@inertiajs/vue3'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'
import TicketBadge from '@/Components/TicketBadge.vue'
import { router } from '@inertiajs/vue3'

const props = defineProps({
    tickets: Object,
    filters: Object,
    statuses: Array,
    priorities: Array,
    categories: Array,
})

function applyFilters(field, event) {
    router.get(route('tickets.index'), {
        ...props.filters,
        [field]: event.target.value,
    }, {
        preserveState: true,
    })
}

function destroy(ticket) {
    if (confirm(`Delete "${ticket.title}"? This cannot be undone.`)) {
        router.delete(route('tickets.destroy', ticket.id))
    }
}
</script>

<template>
    <AuthenticatedLayout>
        <template #header>
            <div class="flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between">
                <h2 class="text-xl font-semibold text-gray-900">My Tickets</h2>
                <a
                    :href="route('tickets.create')"
                    class="inline-flex items-center rounded-lg bg-indigo-600 px-4 py-2 text-sm font-semibold text-white hover:bg-indigo-500"
                >
                    New Ticket
                </a>
            </div>
        </template>

        <div class="py-8">
            <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
                <div class="mb-5 grid gap-3 sm:grid-cols-3">
                    <select
                        :value="filters.status"
                        @change="applyFilters('status', $event)"
                        class="rounded-lg border-gray-300"
                    >
                        <option value="">All statuses</option>
                        <option v-for="status in statuses" :key="status" :value="status">
                            {{ status }}
                        </option>
                    </select>

                    <select
                        :value="filters.priority"
                        @change="applyFilters('priority', $event)"
                        class="rounded-lg border-gray-300"
                    >
                        <option value="">All priorities</option>
                        <option v-for="priority in priorities" :key="priority" :value="priority">
                            {{ priority }}
                        </option>
                    </select>

                    <select
                        :value="filters.category"
                        @change="applyFilters('category', $event)"
                        class="rounded-lg border-gray-300"
                    >
                        <option value="">All categories</option>
                        <option v-for="category in categories" :key="category" :value="category">
                            {{ category }}
                        </option>
                    </select>
                </div>

                <div v-if="tickets.data.length === 0" class="rounded-xl border border-dashed border-gray-300 bg-white p-10 text-center">
                    <h3 class="text-lg font-semibold text-gray-900">No tickets yet</h3>
                    <p class="mt-1 text-sm text-gray-600">
                        Create your first support ticket to start tracking requests.
                    </p>
                </div>

                <div v-else class="space-y-4">
                    <article
                        v-for="ticket in tickets.data"
                        :key="ticket.id"
                        class="rounded-xl border border-gray-200 bg-white p-5 shadow-sm"
                    >
                        <div class="flex flex-col gap-3 sm:flex-row sm:items-start sm:justify-between">
                            <div>
                                <h3 class="font-semibold text-gray-900">
                                    <a :href="route('tickets.show', ticket.id)" class="hover:text-indigo-600">
                                        {{ ticket.title }}
                                    </a>
                                </h3>
                                <p class="mt-1 line-clamp-2 text-sm text-gray-600">
                                    {{ ticket.description }}
                                </p>
                                <p class="mt-2 text-xs text-gray-500">
                                    {{ ticket.category }} • {{ ticket.created_at }}
                                </p>
                            </div>

                            <div class="flex flex-wrap items-center gap-2">
                                <TicketBadge :label="ticket.priority" />
                                <TicketBadge :label="ticket.status" />
                            </div>
                        </div>

                        <div class="mt-4 flex flex-wrap gap-2">
                            <a
                                :href="route('tickets.edit', ticket.id)"
                                class="rounded-lg border border-gray-300 px-3 py-1.5 text-sm hover:bg-gray-50"
                            >
                                Edit
                            </a>
                            <button
                                type="button"
                                @click="destroy(ticket)"
                                class="rounded-lg border border-red-300 px-3 py-1.5 text-sm text-red-600 hover:bg-red-50"
                            >
                                Delete
                            </button>
                        </div>
                    </article>

                    <nav v-if="tickets.links.length > 3" class="flex flex-wrap gap-2">
                        <component
                            :is="link.url ? 'a' : 'span'"
                            v-for="(link, index) in tickets.links"
                            :key="index"
                            :href="link.url"
                            v-html="link.label"
                            class="rounded-lg border px-3 py-1.5 text-sm"
                            :class="link.active
                                ? 'border-indigo-600 bg-indigo-600 text-white'
                                : 'border-gray-300 bg-white text-gray-700'"
                        />
                    </nav>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>