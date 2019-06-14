<template>
    <div class="btn-group" role="group" aria-label="pagination">
        <button type="button" class="btn btn-outline-secondary" :disabled="current === 1" @click="onClick(1)">
            <FontAwesomeIcon icon="angle-double-left" />
        </button>
        <button
                type="button"
                v-for="(page, index) in pages"
                :key="index"
                :class="classes(page)"
                @click="onClick(page)"
        >
            {{ page }}
        </button>
        <button type="button" class="btn btn-outline-secondary" :disabled="current === total" @click="onClick(total)">
            <FontAwesomeIcon icon="angle-double-right" />
        </button>
    </div>
</template>

<script>
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

    export default {
        components: { FontAwesomeIcon },
        props: {
            total:   { type: Number, required: true },
            current: { type: Number, required: true }
        },
        computed: {
            pages() {
                const items = Math.min(this.total, 5);
                const array = Array.from(Array(items).keys()).map(item => item - Math.floor(items/2));
                const pages = array
                    .map(item => item + this.current)
                    .map(item => item % this.total)
                    .map(item => item < 0 ? item + 5 : item)
                    .map(item => item + 1)
                    .sort();

                return [...new Set(pages)];
            }
        },
        methods: {
            classes(index) {
                return index === this.current ? 'btn btn-primary' : 'btn btn-outline-primary';
            },
            onClick(page) {
                this.$emit('click', page)
            }
        }
    };
</script>