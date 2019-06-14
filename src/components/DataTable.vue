<template>
    <Card>
        <template v-slot:header>
            <input
                    type="search"
                    class="datatable_search-field"
                    placeholder="Rechercher"
                    v-model="filter"
                    @input="onFilterChange"
            />
        </template>
        <Table
                :columns="columns"
                :rows="rows"
                :sort="sort"
                :direction="direction"
                v-on="$listeners"
                @sort="onSort"
        />
        <template v-slot:footer>
            <DataControls
                    href="controls"
                    :current-page="meta.currentPage"
                    :from="meta.from"
                    :last-page="meta.lastPage"
                    :per-page="meta.perPage"
                    :to="meta.to"
                    :total="meta.total"
                    @change="onChange"
            />
        </template>
    </Card>
</template>

<script>
    import axios from 'axios';
    import Table from './Table';
    import Card from './Card';
    import DataControls from './DataControls';

    export default {
        components: { Card, DataControls, Table },
        props: {
            columns: { type: Array,    required: true },
            url:     { type: String,   required: true },
            axios:   { type: Function, required: false, default: axios },
        },
        data() {
            return {
                rows: [],
                filter: null,
                meta: {
                    currentPage: 1,
                    from: null,
                    lastPage: 1,
                    perPage: 10,
                    to: null,
                    total: 0
                },
                sort: null,
                direction: null,
            };
        },
        mounted() {
            this.onChange();
        },
        methods: {
            onChange({ page, perPage } = {}) {
                this.axios.get(this.url, {
                    params: {
                        page: page,
                        per_page: perPage,
                        filter: this.filter,
                        order_by: this.sort,
                        direction: this.direction
                    }
                }).then(response => {
                    // update the table meta
                    const meta = response.data.meta;
                    this.meta = {
                        currentPage: meta.current_page,
                        from: meta.from,
                        lastPage: meta.last_page,
                        perPage: parseInt(meta.per_page),
                        to: meta.to,
                        total: meta.total,
                    };
                    // get the rows from the request
                    this.rows = response.data.data;
                })
            },
            onFilterChange() {
                this.onChange({ page: this.meta.currentPage, perPage: this.meta.perPage })
            },
            onSort(column) {
                if (this.sort === column) {
                    this.direction = (this.direction === 'asc' ? 'desc' : 'asc');
                }
                else {
                    this.sort = column;
                    this.direction = 'asc';
                }
                this.onChange();
            }
        }
    };
</script>
