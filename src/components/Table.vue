<template>
    <table class="table datatable_table">
        <thead>
        <tr>
            <th scope="col" class="sortable" v-for="column in columns" :key="column.name">
                {{ column.header }}
                <FontAwesomeIcon
                        v-if="column.sortable"
                        :icon="column.name === sort ? (direction === 'asc' ? 'sort-down': 'sort-up') : 'sort'"
                        @click="onClick(column.name)"
                />
            </th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="row in rows" :key="row.id">
            <td v-for="column in columns" :key="column.name">
                <component v-if="column.transformer" :is="column.transformer" v-bind="row" v-on="$listeners">
                    {{ row[column.name] }}
                </component>
                <template v-else>
                    {{ row[column.name] }}
                </template>
            </td>
        </tr>
        </tbody>
    </table>
</template>

<script>
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

    export default {
        components: { FontAwesomeIcon },
        props: {
            columns:   { type: Array,  required: true },
            rows:      { type: Array,  required: true },
            sort:      { type: String, required: false, default: null },
            direction: { type: String, required: false, default: null }
        },
        methods: {
            onClick(column) {
                this.$emit('sort', column)
            }
        }
    }
</script>