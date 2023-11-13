<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
    fill: {
        type: String,
        default: 'base',
        validator(value) {
            return ['base', 'line', 'lineLight', 'lineTheme', 'text', 'textTheme', 'colorLight'].includes(value);
        },
    },
    state: { type: String, default: 'theme' },
    radius: { type: String, default: 'base' },
    size: { type: String, default: 'big' },
    line: { type: String, default: 'solid' },
    injClass: { type: String, default: '' },
    love: { type: Boolean, default: false },
    heightOut: { type: String, default: '2' },
    heightIn: { type: String, default: '3' },
    group: { type: Boolean, default: false },
    disabled: { type: Boolean, default: false },
    customSize: { type: Boolean, default: false },
    customWidth: { type: Number, default: 0 },
    customHeight: { type: Number, default: 0 },
});

const { fill, state, radius, size, line, injClass, love, heightOut, heightIn, group, disabled, customSize, customWidth, customHeight } =
    props;

const stateObj = ref({
    theme: 'bg-primary dark:bg-dark',
    success: 'bg-success',
    warning: 'bg-warning',
    error: 'bg-error',
    info: 'bg-info',
});

const radiusObj = ref({ none: 'rounded-none', base: 'rounded', xl: 'rounded-xl', '2xl': 'rounded-2xl', full: 'rounded-full' });

const fillObj = ref({
    base: '',
    line: 'border border-black dark:border-white !text-black dark:!text-white',
    lineLight: 'border border-black/20 dark:border-white/30 !text-black dark:!text-white',
    lineTheme: 'border border-primary dark:border-dark !text-primary dark:!text-dark',
    text: '!text-black dark:!text-white',
    textTheme: '!text-primary dark:!text-dark',
    colorLight: '!bg-black/5 dark:!bg-white/10 !text-black dark:!text-white',
});

const sizeObj = ref({ full: 'w-full', big: 'w-full', md: 'w-1/2', sm: 'w-1/4', auto: 'w-auto' });

const lineObj = ref({ solid: 'border-solid', dashed: 'border-dashed', dotted: 'border-dotted' });

const heightOutObj = ref({ 0: 'py-0', 1: 'py-1', 2: 'py-2', 3: 'py-3', 4: 'py-4' });

const heightInObj = ref({ 0: 'py-0', 1: 'py-1', 2: 'py-2', 3: 'py-3', 4: 'py-4' });

const block = computed(() => (size === 'full' || size === 'big') && !customSize);

const textColor = computed(() => (state === 'theme' ? 'text-white dark:text-black' : 'text-white'));
</script>

<template>
    <div
        :class="[
            heightOutObj[heightOut] || heightOutObj['2'],
            size === 'big' && !customSize ? 'px-4' : 'px-0',
            block ? 'block' : 'inline',
            love ? 'text-xl' : '',
        ]"
    >
        <button
            @click="handleClick"
            :class="[
                'truncate',
                !group && !disabled ? 'active:opacity-80' : '',
                heightInObj[heightIn] || heightInObj['3'],
                sizeObj[size] || sizeObj.big,
                textColor,
                lineObj[line] || lineObj.solid,
                radiusObj[radius] || radiusObj.base,
                fill === 'base' && (stateObj[state] || stateObj.theme),
                fillObj[fill] || fillObj.base,
                injClass,
                disabled ? 'cursor-not-allowed opacity-50' : '',
            ]"
            :disabled="disabled"
            :style="customSize ? `width:${customWidth}px;height:${customHeight}px;padding:0;` : ''"
        >
            <slot />
        </button>
    </div>
</template>
