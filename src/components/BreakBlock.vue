<template>
    <div class="box has-text-centered is-centered" 
         @click="updateValue($event)">
        <p class="title is-3">Break Length</p>
        <p class="time is-size-3">{{ breakLength }}</p>
        <div class="buttons is-centered">
            <button class="button" name="minus" :disabled="isActive()">-</button>
            <button class="button" name="plus" :disabled="isActive()">+</button>
        </div>
    </div>  
</template>

<script>
    export default {
        props: {
            breakLength: {
                type: Number,
                required: true
            }
        },
        methods: {
            updateValue($event) {
                this.$emit('update', $event.target.name)
            }, 
            isActive() {
                return this.$parent.clock.isActive;
            }
        }
    }
</script>

<style scoped>
    .break-time {
        padding: 5px;
    }

    .b-buttons {
        display: flex;
    }

    @media all and (max-width: 425px) {
        .is-3 {
            font-size: 1em;
        }

        .box {
            width: 48%;
        }
    }
</style>
