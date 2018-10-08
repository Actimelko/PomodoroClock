<template>
    <div class="box display"
        @click="startTimer($event)">
        <div class="has-text-centered">
           <p class="title is-3">Session</p>
           <p class="is-size-2">{{ timer }}</p>
        </div>  
        <div class="has-text-centered">
            <button class="button" name="start">start</button>
            <button class="button" name="pause">pause</button>
            <button class="button" name="refresh">refresh</button>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            timer: {
                type: String,
                required: true
            }
        },
        methods:{
            startTimer($event){
                this.$emit('start', $event.target.name);
            }
        }
    }
</script>

<style>
    .display {
        width: 45%;
        margin: 0 auto;
    }
</style>


