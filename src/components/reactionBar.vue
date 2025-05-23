<template>
    <div class="flex justify-between p-2">
        <span class="text-sm">{{ '&#128077;' }} {{ likes }}</span>
        <span class="text-sm">{{ '&#128588;' }} {{ laughs }}</span>
        <span class="text-sm">{{ '&#128151;' }} {{ hearts }}</span>
        <span class="text-sm">{{ '&#128542;' }} {{ cries }}</span>
        <span class="text-sm">{{ '&#9889;' }} {{ tippedAmount }}</span>
    </div>
</template>

<script>
export default {
    name: 'ReactionBar',
    props: {
        likes: {
            type: Number,
            required: true,
        },
        laughs: {
            type: Number,
            required: true,
        },
        hearts: {
            type: Number,
            required: true,
        },
        cries: {
            type: Number,
            required: true,
        },
        tippedAmount: {
            type: Number,
            required: true,
        },
    },
}
</script>