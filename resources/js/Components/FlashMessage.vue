<script setup>
import { ref, watch } from 'vue'
import { usePage } from '@inertiajs/vue3'

const page = usePage()
const visible = ref(false)
const message = ref('')
const type = ref('success')

watch(
    () => page.props.flash,
    (flash) => {
        if (flash?.success || flash?.error) {
            message.value = flash.success ?? flash.error
            type.value = flash.success ? 'success' : 'error'
            visible.value = true

            setTimeout(() => (visible.value = false), 5000)
        }
    },
    { immediate: true }
)
</script>

<template>
    <transition
        enter-active-class="transition ease-out duration-200"
        enter-from-class="opacity-0 -translate-y-2"
        leave-active-class="transition ease-in duration-150"
        leave-to-class="opacity-0 -translate-y-2"
    >
        <div
            v-if="visible"
            class="fixed inset-x-4 top-4 z-50 mx-auto max-w-md rounded-lg border p-4 shadow-lg sm:inset-x-auto sm:right-6"
            :class="type === 'success'
                ? 'border-emerald-200 bg-emerald-50 text-emerald-800'
                : 'border-red-200 bg-red-50 text-red-800'"
        >
            <div class="flex items-start justify-between gap-3">
                <p class="text-sm font-medium">{{ message }}</p>
                <button type="button" @click="visible = false" class="text-sm">×</button>
            </div>
        </div>
    </transition>
</template>