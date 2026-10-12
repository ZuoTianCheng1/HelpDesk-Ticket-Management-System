<script setup>
defineProps({
    label: String,
    tone: { type: String, default: 'gray' },
})

const tones = {
    Low: 'bg-green-100 text-green-800',
    Medium: 'bg-yellow-100 text-yellow-800',
    High: 'bg-orange-100 text-orange-800',
    Urgent: 'bg-red-100 text-red-800',
    Open: 'bg-blue-100 text-blue-800',
    'In Progress': 'bg-indigo-100 text-indigo-800',
    Resolved: 'bg-emerald-100 text-emerald-800',
    Closed: 'bg-gray-200 text-gray-700',
}
</script>

<template>
    <span
        class="inline-flex rounded-full px-2.5 py-1 text-xs font-semibold"
        :class="tones[label] ?? 'bg-gray-100 text-gray-700'"
    >
        {{ label }}
    </span>
</template>