<template>
    <div class="card datatable_card">
        <div class="card-header">
            <slot name="header" />
        </div>
        <slot />
        <div class="card-footer">
            <slot name="footer" />
        </div>
    </div>
</template>

<script>
    export default {};
</script>