<script setup>
import { useForm } from '@inertiajs/vue3'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue'

const props = defineProps({
    statuses: Array,
    priorities: Array,
    categories: Array,
})

const form = useForm({
    title: '',
    description: '',
    priority: 'Medium',
    status: 'Open',
    category: 'Other',
})

function submit() {
    form.post(route('tickets.store'))
}
</script>

<template>
    <AuthenticatedLayout>
        <template #header>
            <h2 class="text-xl font-semibold text-gray-900">Create Ticket</h2>
        </template>

        <div class="py-8">
            <div class="mx-auto max-w-3xl px-4 sm:px-6 lg:px-8">
                <form @submit.prevent="submit" class="space-y-5 rounded-xl bg-white p-6 shadow-sm">
                    <div>
                        <label class="block text-sm font-medium text-gray-700">Title</label>
                        <input v-model="form.title" type="text" class="mt-1 w-full rounded-lg border-gray-300" />
                        <p v-if="form.errors.title" class="mt-1 text-sm text-red-600">{{ form.errors.title }}</p>
                    </div>

                    <div>
                        <label class="block text-sm font-medium text-gray-700">Description</label>
                        <textarea v-model="form.description" rows="5" class="mt-1 w-full rounded-lg border-gray-300"></textarea>
                        <p v-if="form.errors.description" class="mt-1 text-sm text-red-600">{{ form.errors.description }}</p>
                    </div>

                    <div class="grid gap-4 sm:grid-cols-3">
                        <div>
                            <label class="block text-sm font-medium text-gray-700">Priority</label>
                            <select v-model="form.priority" class="mt-1 w-full rounded-lg border-gray-300">
                                <option v-for="priority in priorities" :key="priority" :value="priority">
                                    {{ priority }}
                                </option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-sm font-medium text-gray-700">Status</label>
                            <select v-model="form.status" class="mt-1 w-full rounded-lg border-gray-300">
                                <option v-for="status in statuses" :key="status" :value="status">
                                    {{ status }}
                                </option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-sm font-medium text-gray-700">Category</label>
                            <select v-model="form.category" class="mt-1 w-full rounded-lg border-gray-300">
                                <option v-for="category in categories" :key="category" :value="category">
                                    {{ category }}
                                </option>
                            </select>
                        </div>
                    </div>

                    <button
                        type="submit"
                        :disabled="form.processing"
                        class="rounded-lg bg-indigo-600 px-4 py-2 text-sm font-semibold text-white disabled:opacity-50"
                    >
                        {{ form.processing ? 'Saving…' : 'Create Ticket' }}
                    </button>
                </form>
            </div>
        </div>
    </AuthenticatedLayout>
</template>