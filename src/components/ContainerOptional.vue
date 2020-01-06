<template>
    <div class="containerOptional">
        <div
            class="containerOptional__vl"
            :style="{'background-color': localEnabled?color:'#414042'}"
        ></div>
        <div class="containerOptional__header">
            <span class="header__name" :style="{'color':localEnabled?color:'#414042'}">{{name}}</span>
            <ToggleSwitch
                v-if="isOptional"
                class="header__toggle"
                v-model="localEnabled"
                :textOff="'不開放'"
                :textOn="'開放'"
            ></ToggleSwitch>
        </div>
        <div class="containerOptional__row">
            <slot></slot>
        </div>
    </div>
</template>
<script>
export default {
    data: () => ({
        localEnabled: true
    }),
    props: {
        isOptional: {
            type: Boolean,
            default: false
        },
        value: {
            type: Object,
            default: () => {
                return {}
            }
        },
        name: {
            type: String,
            default: ''
        },
        enabled: {
            type: Boolean,
            default: true
        },
        fields: {
            type: Array,
            default: () => {
                return []
            }
        },
        color: {
            type: String,
            default: '#00b85a'
        },
    },
    mounted() {
        this.fields.forEach(key => {
            const targetValue = this.value[key]
            // 這樣的轉換將0視為true, null undefined視為false
            if (!~~targetValue) {
                this.localEnabled = true
            }
        })
    },
    watch: {
        enabled() {
            this.localEnabled = this.enabled
        },
        localEnabled(newValue) {
            if (newValue === false) {
                this.fields.forEach(key => {
                    this.value[key] = ""
                })
            }
        }
    }
}
</script>
<style lang="scss" scoped>
.containerOptional {
    background-color: #f0eff1;
    padding: 8px 13px;
    position: relative;
    .containerOptional__vl {
        position: absolute;
        left: 0;
        top: 50%;
        transform: translateY(-50%);
        height: calc(100% - 16px);
        width: 5px;
        transition: all 0.3s;
    }
    .containerOptional__header {
        margin-bottom: 12px;
        display: flex;
        transition: all 0.3s;
        > * {
            align-self: center;
        }
        .header__toggle {
            margin: 0 0 0 16px;
        }
    }
    .containerOptional__row {
        display: flex;
        flex-wrap: wrap;
        > *:not(:last-child) {
            margin-right: 24px;
        }
    }
}
</style>