<template>
    <div class="card">
        <div class="card-body">
        <input v-model="form.title" class="form-control" type="text">
        </div>
        <div class="card-footer">
        <button @click="cancel">Cancel</button>
        <button @click="submit">{{ submitText }}</button>
        </div>
    </div>
</template>

<script>
export default {
    props: [ 'submitText', 'item' ],
    data () {
        return {
            form: {
                title: ''
            }
        }
    },
    mounted () {
        if (this.item) {
            this.form.title = this.item.title
        }
    },
    methods: {
        submit () {
            this.$emit('submit', this.form)
            this.form.title = ''
        },
        cancel () {
            this.$emit('cancel')
        }
    }
}
</script>
