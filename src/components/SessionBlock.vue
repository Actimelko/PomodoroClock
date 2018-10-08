<template>
    <div class="box has-text-centered is-centered"
         @click="updateValue($event)">
        <h3 class="title is-3">Session Length</h3>
        <p class="time is-size-3">{{ sessionLength }}</p>
        <div class="buttons is-centered">
            <button class="button" name="minus" :disabled="isActive()">-</button>
            <button class="button" name="plus" :disabled="isActive()">+</button>
        </div>
    </div>  
</template>

<script>
    export default {
        props: {
            sessionLength: {
                type: Number,
                required: true
            }
        },
        methods: {
            updateValue($event) {
                this.$emit('update', $event.target.name);
            }, 
            isActive() {
                return this.$parent.clock.isActive;
            }
        }
    }
</script>

<style scoped>
    @media all and (max-width: 425px) {
        .is-3 {
            font-size: 1em;
        }

        .box {
            width: 48%;
        }
    }
</style>