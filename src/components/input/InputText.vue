<template>
    <label :ref="`inputGroup${id}`" class="inputGroup">
        <span class="inputGroup__name" :class="{'inputGroup__name--required': required}">{{name}}</span>
        <input
            v-if="!disabled"
            class="inputGroup__input"
            :value="value"
            @input="$emit('input', $event.target.value)"
        />
        <input
            v-else
            :disabled="true"
            :class="{'inputGroup__input--disabled': disabled}"
            class="inputGroup__input"
            :value="value"
            :readonly="value"
        />
    </label>
</template>
<script>
const { uuid } = require('uuidv4');
import { handleArrowMixin } from '@/components/Nav/arrowUp.js'
import { widthMixins } from './_inputMixins'
export default {
    mixins: [handleArrowMixin, widthMixins],
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
        disabled: {
            type: Boolean,
            default: false
        },
        validate: {
            type: Function,
            default: function() {
                return ''
            }
        }
    },
    computed: {
        id() {
            return uuid()
        },
    },
    mounted() {
        const targetWidth = this.widths[this.width]
        const inputLabel = this.$refs[`inputGroup${this.id}`]
        inputLabel.style.minWidth = targetWidth
    },
    methods: {

    }
}
</script>
<style lang="scss" scoped src="./_input.scss">
</style>