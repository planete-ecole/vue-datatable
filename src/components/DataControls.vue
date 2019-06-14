<template>
    <div class="row row-eq-height datatable_controls">
        <div class="col-md-5 align-self-center">
            <label class="datatable_row-count-label">
                Afficher
                <select class="custom-select" v-model="selection" @change="onChange">
                    <option value="10">10</option>
                    <option value="25">25</option>
                    <option value="50">50</option>
                    <option value="100">100</option>
                </select>
                éléments
            </label>
        </div>
        <div class="col-md-2 align-self-center">
            <span>{{ from }} - {{ to }}</span>
        </div>
        <div class="col-md-5 align-self-center">
            <Pagination :total="lastPage" :current="currentPage" @click="onClick" />
        </div>
    </div>
</template>

<script>
    import Pagination from './Pagination';

    export default {
        components: { Pagination },
        props: {
            currentPage: { type: Number, required: true },
            lastPage:    { type: Number, required: true },
            perPage:     { type: Number, required: true },
            total:       { type: Number, required: true },
            from:        { type: [Number, String], required: false, default: '-' },
            to:          { type: [Number, String], required: false, default: '-' },
        },
        data() {
            return {
                selection: this.perPage,
            }
        },
        methods: {
            onClick(page) {
                this.$emit('change', { page: page, perPage: this.selection })
            },
            onChange() {
                this.onClick(this.currentPage);
            }
        },
    };
</script>
