<template>
    <aside>
        <span>{{startDate}}</span>
        tot
        <span>{{endDate}}</span>

    </aside>
</template>

<script>
import { format, add } from 'date-fns';

export default {
    name: "date-interval",
    computed: {
        startDate () {
            return format(new Date(), 'dd/MM/yyyy');
        },
        endDate () {
            return format(add(new Date(), {days: 7}), 'dd/MM/yyyy');
        }
    }
}
</script>

<style lang="scss" scoped>
aside {
    display: inline-block;

    span {
        position: relative;
        border: 1px solid #000;
        padding: 10px;
        color: white;
        background: #555;
        z-index: 5;
    }
}
</style>