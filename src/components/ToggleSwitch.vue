<template>
    <label class="switch" :style="{width:`${width}px`,height:`${height}px`}">
        <span
            class="slider round"
            :ref="sliderId"
            :style="{'line-height':`${sliderSize}px`,'background-color': _value?sliderColor:'#8794a7'}"
        >
            <span
                v-show="!_value"
                class="slider__text slider__text--right"
                :ref="textOffId"
            >{{textOff}}</span>
            <span v-show="_value" class="slider__text slider__text--left" :ref="textOnId">{{textOn}}</span>
            <button
                @click="_value=!_value"
                class="slider__button"
                :ref="sliderButtonId"
                :style="{width:`${sliderSize}px`,height:`${sliderSize}px`, left:`${padding}px`, bottom:`${padding}px`}"
            ></button>
        </span>
    </label>
</template>
<script>
const { uuid } = require('uuidv4');
export default {
    name: "toggle-switch",
    props: {
        value: {
            type: Boolean,
            default: false
        },
        textOff: {
            type: String,
            default: ""
        },
        textOn: {
            type: String,
            default: ""
        },
        textSize: {
            type: String,
            default: "14"
        },
        width: {
            type: String,
            default: "72"
        },
        height: {
            type: String,
            default: "24"
        },
        sliderSize: {
            type: String,
            default: "26"
        },
        sliderColor: {
            type: String,
            default: '#59B201'
        }
    },
    mounted() {
        const textOff = this.$refs[this.textOffId];
        const paddingTop = (this.height - this.sliderSize) / 2;
        textOff.style.fontSize = `${this.textSize}px`;
        textOff.style.top = `${paddingTop}px`;
        const textOn = this.$refs[this.textOnId];
        textOn.style.fontSize = `${this.textSize}px`;
        textOn.style.top = `${paddingTop}px`;
        this.setButtonPosition();
    },
    computed: {
        id() {
            return uuid()
        },
        _value: {
            get() {
                return this.value;
            },
            set(newValue) {
                this.$emit('input', newValue)
            }
        },
        sliderId() {
            return `slider-${this.id}`;
        },
        sliderButtonId() {
            return `sliderButton-${this.id}`;
        },
        textOnId() {
            return `textOn-${this.id}`;
        },
        textOffId() {
            return `textOff-${this.id}`;
        },
        padding() {
            return (this.height - this.sliderSize) / 2;
        },
        transformValue() {
            const translateValue =
                this.width - this.padding * 2 - this.sliderSize;
            const transformString = `translateX(${translateValue}px)`;
            return transformString;
        }
    },
    watch: {
        value() {
            this._value = this.value;
            this.$nextTick(() => {
                this.setButtonPosition();
            });
        }
    },
    methods: {
        setButtonPosition() {
            const sliderButton = this.$refs[this.sliderButtonId];
            if (this._value) {
                sliderButton.style.transform = this.transformValue;
            } else {
                sliderButton.style.transform = "";
            }
        },
    }
};
</script>

<style lang="scss" scoped>
.switch {
    position: relative;
    display: inline-block;
    cursor: pointer;
}

.switch input {
    opacity: 0;
    width: 0;
    height: 0;
}

.slider {
    position: absolute;

    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    -webkit-transition: 0.4s;
    transition: 0.4s;
    .slider__text {
        color: white;
        position: absolute;
    }
    .slider__text--left {
        left: 8px;
    }
    .slider__text--right {
        right: 8px;
    }
}

.slider__button {
    position: absolute;
    content: "";
    background-color: white;
    -webkit-transition: 0.4s;
    transition: 0.4s;
    border-radius: 34px;
}

.slider.round {
    border-radius: 34px;
}
</style>
