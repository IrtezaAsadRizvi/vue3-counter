<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
interface counterConfig {
    inputColor?: string;
    countUpColor?: string;
    countDownColor?: string;
    textColor?: string;
    initial?: number;
    max?: number;
    min?: number;
    arrow?: Boolean;
    vertical?: Boolean;
}
const emit = defineEmits(['countUp', 'countDown', 'input', 'change'])

const props: any = defineProps({
    config: {
        type: Object as () => counterConfig,
        default: () => ({
            inputColor: '#212121',
            countUpColor: '#212121',
            countDownColor: '#212121',
            textColor: '#FFFFFF',
            initial: 0,
            arrow: false,
            vertical: false
        } as counterConfig)
    }
})
let count = ref(props.config.initial || 0)
const counterInput = ref('');

const countUp = (event: any) => {
    count.value = (!props.config.max || count.value < parseInt(props.config.max)) ? count.value + 1 : count.value;
    propagateEvent('countUp', {...event, value: count.value})
}
const countDown = (event: any) => {
    count.value = (!props.config.min || count.value > parseInt(props.config.min)) ? count.value - 1 : count.value;
    propagateEvent('countUp', {...event, value: count.value})
}
const resizeInput = () => {
    const input: any = counterInput.value;
    input.style.width = input.value.length + 'ch';
};
const propagateEvent = (name: any, event: any) => {
    emit(name, {...event, value: count.value})
}
const handleChange = () => {
    if(props.config.max && count.value > parseInt(props.config.max)) {
        count.value = parseInt(props.config.max)
        return
    }
    if(props.config.min && count.value < parseInt(props.config.min)) {
        count.value = parseInt(props.config.min)
        return
    }
    resizeInput();
    propagateEvent('change', {value: count.value})
}
onMounted(() => {
    resizeInput();
});

watch(count, handleChange);

</script>

<template>
    <div class="counter" :class="{'counter--vertical': props.config.vertical}">
        <button class="counter__down"
            :disabled="props.config.min == count"
            :style="{ color: props.config.countDownColor }"
            @click="countDown($event)">
            <svg v-if="props.config.arrow && props.config.vertical" viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="6 9 12 15 18 9"></polyline></svg>
            <svg v-else-if="props.config.arrow" viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="15 18 9 12 15 6"></polyline></svg>
            <svg v-else viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><line x1="5" y1="12" x2="19" y2="12"></line></svg>
        </button>
        <input class="counter__input" 
            ref="counterInput" 
            type="number"
            @keyup="propagateEvent('input', $event)"
            :style="{ backgroundColor: props.config.inputColor, color : props.config.textColor }"
            v-model="count">
        <button class="counter__up" 
            :disabled="props.config.max == count"
            :style="{ color: props.config.countUpColor }" 
            @click="countUp($event)">
            <svg v-if="props.config.arrow && props.config.vertical"  viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="18 15 12 9 6 15"></polyline></svg>
            <svg v-else-if="props.config.arrow" viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="9 18 15 12 9 6"></polyline></svg>
            <svg v-else viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
        </button>
    </div>
</template>

<style scoped>
.counter {
    display: flex;
    align-items: center;
}
.counter--vertical {
    flex-direction: column-reverse;
}
.counter__up,
.counter__down {
    background-color: transparent;
    border: none;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.15s ease-in-out;
    opacity: 0.7;
}
.counter__up:disabled,
.counter__down:disabled {
    opacity: 0.3;
    pointer-events: none;
}
.counter__up:hover,
.counter__down:hover,
.counter__up:active,
.counter__down:active {
    transform: scale(1.2,1.2);
    opacity: 1;
}
.counter__up svg,
.counter__down svg {
    pointer-events: none;
    position: relative;
    top: 2px;
}
.counter__input {
    height: 60px;
    min-width: 60px;
    border-radius: 10px;
    text-align: center;
    border: none;
    font-size: 24px;
    font-weight: bold;
    transition: all 0.15s ease-in-out;
}
.counter__input:active,
.counter__input:hover,
.counter__input:focus {
    outline: none;
    border: none;
    box-shadow: none;
    transform: scale(1.05,1.05)
}
.counter__input:active,
.counter__input:focus {
    filter: brightness(105%);
}
.counter__input::-webkit-outer-spin-button,
.counter__input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}
.counter__input[type=number] {
    -moz-appearance: textfield;
}
</style>
