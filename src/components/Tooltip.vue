<script setup>
import { ref, onMounted, onUnmounted, nextTick, watch } from 'vue';

const props = defineProps({
  text: {
    type: String,
    required: true
  },
  position: {
    type: String,
    default: 'right',
    validator: (val) => ['top', 'right', 'bottom', 'left'].includes(val)
  },
  delay: {
    type: Number,
    default: 300
  }
});

const isVisible = ref(false);
const triggerRef = ref(null);
const tooltipStyle = ref({});
let showTimeout = null;

const calculatePosition = () => {
  if (!triggerRef.value) return;
  
  const trigger = triggerRef.value.getBoundingClientRect();
  const offset = 8;
  
  let top, left;
  
  switch (props.position) {
    case 'right':
      top = trigger.top + trigger.height / 2;
      left = trigger.right + offset;
      break;
    case 'left':
      top = trigger.top + trigger.height / 2;
      left = trigger.left - offset;
      break;
    case 'top':
      top = trigger.top - offset;
      left = trigger.left + trigger.width / 2;
      break;
    case 'bottom':
      top = trigger.bottom + offset;
      left = trigger.left + trigger.width / 2;
      break;
  }
  
  tooltipStyle.value = {
    top: `${top}px`,
    left: `${left}px`
  };
};

const show = async () => {
  showTimeout = setTimeout(async () => {
    await nextTick();
    calculatePosition();
    isVisible.value = true;
  }, props.delay);
};

const hide = () => {
  if (showTimeout) clearTimeout(showTimeout);
  isVisible.value = false;
};

onUnmounted(() => {
  if (showTimeout) clearTimeout(showTimeout);
});
</script>

<template>
  <div 
    class="inline-flex"
    @mouseenter="show"
    @mouseleave="hide"
    @focus="show"
    @blur="hide"
    ref="triggerRef"
  >
    <slot></slot>
    
    <Teleport to="body">
      <Transition
        enter-active-class="transition-all duration-200 ease-out"
        enter-from-class="opacity-0 scale-90"
        enter-to-class="opacity-100 scale-100"
        leave-active-class="transition-all duration-150 ease-in"
        leave-from-class="opacity-100 scale-100"
        leave-to-class="opacity-0 scale-90"
      >
        <div 
          v-if="isVisible"
          class="fixed z-[9999] px-3 py-2 text-xs font-medium text-white bg-[#333] rounded-lg shadow-xl border border-white/10 whitespace-nowrap pointer-events-none"
          :class="{
            '-translate-y-1/2': position === 'right' || position === 'left',
            '-translate-x-1/2': position === 'top' || position === 'bottom',
            '-translate-x-full': position === 'left',
            '-translate-y-full': position === 'top'
          }"
          :style="tooltipStyle"
        >
          <!-- Arrow -->
          <div 
            class="absolute w-2 h-2 bg-[#333] border border-white/10 rotate-45"
            :class="{
              '-left-1 top-1/2 -translate-y-1/2 border-r-0 border-t-0': position === 'right',
              '-right-1 top-1/2 -translate-y-1/2 border-l-0 border-b-0': position === 'left',
              '-bottom-1 left-1/2 -translate-x-1/2 border-l-0 border-t-0': position === 'top',
              '-top-1 left-1/2 -translate-x-1/2 border-r-0 border-b-0': position === 'bottom'
            }"
          ></div>
          {{ text }}
        </div>
      </Transition>
    </Teleport>
  </div>
</template>
