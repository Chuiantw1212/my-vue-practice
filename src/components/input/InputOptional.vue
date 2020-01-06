<template>
    <label :ref="`inputGroup${id}`" class="inputGroup">
        <label>
            <input type="checkbox" v-model="localEnabled" />
            {{name}}
        </label>
        <input
            class="inputGroup__input"
            :value="value"
            @keydown="handleArrowUp($event)"
            @input="$emit('input', $event.target.value)"
            :disabled="!localEnabled"
        />
    </label>
</template>
<script>
const { uuid } = require('uuidv4');
import { widthMixins } from './_inputMixins'
export default {
    mixins: [widthMixins],
    data: () => ({
        localEnabled: true
    }),
    props: {
        name: {
            type: String,
            default: ''
        },
        width: {
            type: [String, Number],
            default: '1'
        },
        value: {
            type: [String, Number],
            default: ''
        },
        required: {
            type: Boolean,
            default: false
        },
        enabled: {
            type: Boolean,
            default: true
        },
    },
    computed: {
        id() {
            return uuid()
        },
    },
    watch: {
        enabled() {
            this.localEnabled = this.enabled
        },
        localEnabled(newValue) {
            if (newValue == false) {
                this.$emit('input', '')
            }
        }
    },
    mounted() {
        const targetWidth = this.widths[this.width]
        const inputLabel = this.$refs[`inputGroup${this.id}`]
        inputLabel.style.minWidth = targetWidth
    },
}
</script>
<style lang="scss" scoped src="./_input.scss">
</style>