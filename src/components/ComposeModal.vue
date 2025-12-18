<script setup>
import { ref, watch, nextTick, inject } from 'vue';
import { X, Minus, Maximize2, Minimize2, Paperclip, Image, Link, Smile, Send } from 'lucide-vue-next';

// Inject mobile state
const isMobile = inject('isMobile');

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  replyTo: {
    type: Object,
    default: null
  },
  currentUser: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['close', 'send']);

const isMinimized = ref(false);
const isMaximized = ref(false);
const isVisible = ref(false);
const isClosing = ref(false);

const to = ref('');
const subject = ref('');
const body = ref('');

// Handle open/close with animation
watch(() => props.isOpen, async (newVal) => {
  if (newVal) {
    isVisible.value = true;
    isClosing.value = false;
    await nextTick();
  }
});

const handleClose = () => {
  isClosing.value = true;
  setTimeout(() => {
    isVisible.value = false;
    isClosing.value = false;
    emit('close');
  }, 300);
};

// Pre-fill when replying
watch(() => props.replyTo, (email) => {
  if (email) {
    to.value = email.email;
    subject.value = `Re: ${email.subject}`;
    body.value = `\n\n\n--- Original Message ---\nFrom: ${email.sender} <${email.email}>\nDate: ${email.date} ${email.time}\nSubject: ${email.subject}\n\n${email.body}`;
  } else {
    to.value = '';
    subject.value = '';
    body.value = '';
  }
}, { immediate: true });

const handleSend = () => {
  if (to.value && subject.value) {
    emit('send', {
      to: to.value,
      subject: subject.value,
      body: body.value
    });
    // Reset form
    to.value = '';
    subject.value = '';
    body.value = '';
    handleClose();
  }
};

const toggleMinimize = () => {
  isMinimized.value = !isMinimized.value;
  if (isMinimized.value) isMaximized.value = false;
};

const toggleMaximize = () => {
  isMaximized.value = !isMaximized.value;
  if (isMaximized.value) isMinimized.value = false;
};
</script>

<template>
  <Teleport to="body">
    <!-- Backdrop for maximized state -->
    <Transition
      enter-active-class="transition-opacity duration-300 ease-out"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition-opacity duration-200 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div 
        v-if="isVisible && isMaximized && !isClosing"
        class="fixed inset-0 bg-black/40 backdrop-blur-sm z-40"
        @click="toggleMaximize"
      ></div>
    </Transition>

    <!-- Modal Container -->
    <Transition
      enter-active-class="transition-all duration-300 ease-[cubic-bezier(0.34,1.56,0.64,1)]"
      enter-from-class="opacity-0 translate-y-8 scale-95"
      enter-to-class="opacity-100 translate-y-0 scale-100"
      leave-active-class="transition-all duration-200 ease-in"
      leave-from-class="opacity-100 translate-y-0 scale-100"
      leave-to-class="opacity-0 translate-y-4 scale-95"
    >
      <div 
        v-if="isVisible && !isClosing"
        class="fixed z-50"
        :class="[
          // Position and size transitions
          'transition-all duration-300 ease-[cubic-bezier(0.4,0,0.2,1)]',
          isMobile
            ? 'inset-0'
            : isMaximized 
              ? 'inset-6' 
              : isMinimized 
                ? 'bottom-0 right-6 w-72' 
                : 'bottom-0 right-4 md:right-6 w-full md:w-[560px]'
        ]"
      >
        <div 
          class="bg-[#1E1E1E] shadow-2xl border border-white/10 flex flex-col overflow-hidden transition-all duration-300 ease-[cubic-bezier(0.4,0,0.2,1)]"
          :class="[
            isMaximized 
              ? 'h-full rounded-2xl' 
              : isMinimized 
                ? 'h-auto rounded-t-xl rounded-b-none' 
                : 'h-[520px] rounded-t-xl rounded-b-none'
          ]"
        >
          <!-- Header -->
          <div 
            class="flex items-center justify-between px-4 py-3 bg-gradient-to-r from-[#2B2B2B] to-[#252525] cursor-pointer select-none transition-all duration-200"
            :class="[isMinimized ? 'hover:bg-[#333]' : '']"
            @click="isMinimized && toggleMinimize()"
          >
            <div class="flex items-center gap-2 overflow-hidden">
              <div 
                class="w-2 h-2 rounded-full transition-colors duration-300"
                :class="[to && subject ? 'bg-green-500' : 'bg-gray-500']"
              ></div>
              <span class="text-white text-sm font-medium truncate">
                {{ replyTo ? 'Reply' : 'New Message' }}
                <span v-if="subject && isMinimized" class="text-gray-400"> - {{ subject }}</span>
              </span>
            </div>
            <div class="flex items-center gap-0.5">
              <button 
                @click.stop="toggleMinimize"
                class="p-2 hover:bg-white/10 rounded-lg transition-all duration-200 text-gray-400 hover:text-white group"
                :title="isMinimized ? 'Expand' : 'Minimize'"
              >
                <Minus class="w-4 h-4 transition-transform group-hover:scale-110" />
              </button>
              <button 
                @click.stop="toggleMaximize"
                class="p-2 hover:bg-white/10 rounded-lg transition-all duration-200 text-gray-400 hover:text-white group"
                :title="isMaximized ? 'Restore' : 'Maximize'"
              >
                <Minimize2 v-if="isMaximized" class="w-4 h-4 transition-transform group-hover:scale-110" />
                <Maximize2 v-else class="w-4 h-4 transition-transform group-hover:scale-110" />
              </button>
              <button 
                @click.stop="handleClose"
                class="p-2 hover:bg-red-500/20 rounded-lg transition-all duration-200 text-gray-400 hover:text-red-400 group"
                title="Close"
              >
                <X class="w-4 h-4 transition-transform group-hover:scale-110 group-hover:rotate-90" />
              </button>
            </div>
          </div>

          <!-- Body with smooth height transition -->
          <Transition
            enter-active-class="transition-all duration-300 ease-out"
            enter-from-class="opacity-0 max-h-0"
            enter-to-class="opacity-100 max-h-[600px]"
            leave-active-class="transition-all duration-200 ease-in"
            leave-from-class="opacity-100 max-h-[600px]"
            leave-to-class="opacity-0 max-h-0"
          >
            <div v-show="!isMinimized" class="flex-1 flex flex-col min-h-0 overflow-hidden">
              <!-- From -->
              <div class="px-4 py-2.5 border-b border-white/5 flex items-center gap-2">
                <span class="text-gray-500 text-sm w-12">From</span>
                <div class="flex items-center gap-2">
                  <img :src="currentUser.avatar" class="w-6 h-6 rounded-full ring-1 ring-white/10" />
                  <span class="text-white text-sm">{{ currentUser.name }}</span>
                  <span class="text-gray-500 text-sm">&lt;{{ currentUser.email }}&gt;</span>
                </div>
              </div>

              <!-- To -->
              <div class="px-4 py-2.5 border-b border-white/5">
                <div class="flex items-center gap-2">
                  <span class="text-gray-500 text-sm w-12">To</span>
                  <input 
                    v-model="to"
                    type="email"
                    placeholder="Recipients"
                    class="flex-1 bg-transparent text-white text-sm focus:outline-none placeholder:text-gray-600"
                  />
                </div>
              </div>

              <!-- Subject -->
              <div class="px-4 py-2.5 border-b border-white/5">
                <div class="flex items-center gap-2">
                  <span class="text-gray-500 text-sm w-12">Subject</span>
                  <input 
                    v-model="subject"
                    type="text"
                    placeholder="Subject"
                    class="flex-1 bg-transparent text-white text-sm focus:outline-none placeholder:text-gray-600"
                  />
                </div>
              </div>

              <!-- Body -->
              <div class="flex-1 p-4 min-h-0">
                <textarea 
                  v-model="body"
                  placeholder="Write your message..."
                  class="w-full h-full bg-transparent text-gray-300 text-sm focus:outline-none resize-none placeholder:text-gray-600 leading-relaxed"
                ></textarea>
              </div>

              <!-- Footer -->
              <div class="px-4 py-3 border-t border-white/5 bg-[#1a1a1a] flex items-center justify-between">
                <div class="flex items-center gap-0.5">
                  <button class="p-2.5 text-gray-400 hover:text-white hover:bg-white/5 rounded-lg transition-all duration-200 group" title="Attach file">
                    <Paperclip class="w-5 h-5 transition-transform group-hover:scale-110 group-hover:-rotate-12" />
                  </button>
                  <button class="p-2.5 text-gray-400 hover:text-white hover:bg-white/5 rounded-lg transition-all duration-200 group" title="Insert image">
                    <Image class="w-5 h-5 transition-transform group-hover:scale-110" />
                  </button>
                  <button class="p-2.5 text-gray-400 hover:text-white hover:bg-white/5 rounded-lg transition-all duration-200 group" title="Insert link">
                    <Link class="w-5 h-5 transition-transform group-hover:scale-110" />
                  </button>
                  <button class="p-2.5 text-gray-400 hover:text-white hover:bg-white/5 rounded-lg transition-all duration-200 group" title="Insert emoji">
                    <Smile class="w-5 h-5 transition-transform group-hover:scale-110" />
                  </button>
                </div>
                <button 
                  @click="handleSend"
                  :disabled="!to || !subject"
                  class="flex items-center gap-2 px-5 py-2.5 bg-blue-600 hover:bg-blue-500 disabled:bg-gray-600 disabled:cursor-not-allowed text-white rounded-xl transition-all duration-200 font-medium text-sm hover:shadow-lg hover:shadow-blue-500/25 hover:-translate-y-0.5 active:translate-y-0 group"
                >
                  <Send class="w-4 h-4 transition-transform group-hover:translate-x-0.5" />
                  Send
                </button>
              </div>
            </div>
          </Transition>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
/* Custom easing for smooth morphing */
.ease-morph {
  transition-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1);
}
</style>
