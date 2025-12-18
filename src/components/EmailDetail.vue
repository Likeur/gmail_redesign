<script setup>
import { inject } from 'vue';
import { Archive, Trash, Mail, Folder, MoveRight, Reply, Forward, Paperclip } from 'lucide-vue-next';
import Tooltip from './Tooltip.vue';

const props = defineProps({
  email: {
    type: Object,
    required: true
  }
});

// Inject reply function from App.vue
const openReply = inject('openReply');
const currentUser = inject('currentUser');
const isMobile = inject('isMobile');

const handleReply = () => {
  openReply(props.email);
};
</script>

<template>
  <div class="h-full bg-[#1E1E1E] flex flex-col p-4 md:p-6 overflow-y-auto">
    <!-- Toolbar -->
    <div class="flex items-center justify-between mb-6 text-gray-400">
      <div class="flex gap-1">
        <Tooltip text="Archive" position="bottom">
          <button class="p-2 hover:text-white hover:bg-white/5 rounded-lg transition-colors">
            <Archive class="w-4 h-4"/>
          </button>
        </Tooltip>
        <Tooltip text="Delete" position="bottom">
          <button class="p-2 hover:text-white hover:bg-white/5 rounded-lg transition-colors">
            <Trash class="w-4 h-4"/>
          </button>
        </Tooltip>
        <Tooltip text="Mark as unread" position="bottom">
          <button class="p-2 hover:text-white hover:bg-white/5 rounded-lg transition-colors">
            <Mail class="w-4 h-4"/>
          </button>
        </Tooltip>
        <Tooltip text="Move to folder" position="bottom">
          <button class="p-2 hover:text-white hover:bg-white/5 rounded-lg transition-colors">
            <Folder class="w-4 h-4"/>
          </button>
        </Tooltip>
        <div class="w-px h-4 bg-gray-700 mx-2 self-center"></div>
        <Tooltip text="Forward" position="bottom">
          <button class="p-2 hover:text-white hover:bg-white/5 rounded-lg transition-colors">
            <MoveRight class="w-4 h-4"/>
          </button>
        </Tooltip>
      </div>
    </div>

    <!-- Header -->
    <div class="flex items-start justify-between mb-6">
      <div class="flex-1 min-w-0">
        <div class="flex items-center gap-3 mb-2 flex-wrap">
          <h1 class="text-2xl font-bold text-white">{{ email.subject }}</h1>
          <span v-if="email.folder" class="px-2.5 py-1 bg-[#2B2B2B] text-gray-400 text-xs rounded-full border border-white/5">
            {{ email.folder }}
          </span>
        </div>
      </div>
    </div>

    <!-- Sender Info -->
    <div class="flex items-center justify-between mb-8 pb-6 border-b border-white/5">
      <div class="flex items-center gap-4">
        <img :src="email.avatar" class="w-12 h-12 rounded-full object-cover ring-2 ring-white/10 bg-[#2B2B2B]" />
        <div>
          <h3 class="text-white font-medium">{{ email.sender }}</h3>
          <p class="text-sm text-gray-500">
            To: {{ currentUser.email }}
            <span v-if="email.cc" class="text-gray-600"> · cc: {{ email.cc }}</span>
          </p>
        </div>
      </div>
      <div class="text-sm text-gray-500">
        {{ email.date }} · {{ email.time }}
      </div>
    </div>

    <!-- Content -->
    <div class="text-gray-300 text-sm leading-7 space-y-4 mb-8">
      <p v-for="(paragraph, index) in email.body.split('\n\n')" :key="index" class="whitespace-pre-line">
        {{ paragraph }}
      </p>
    </div>

    <!-- Image Attachment -->
    <div v-if="email.image" class="mb-8 rounded-2xl overflow-hidden">
      <img 
        :src="email.image" 
        :alt="email.subject"
        class="w-full h-auto max-h-80 object-cover"
      />
    </div>

    <!-- Attachment List -->
    <div v-if="email.hasAttachment" class="mb-8 p-4 bg-[#2B2B2B] rounded-xl border border-white/5">
      <div class="flex items-center gap-3">
        <div class="p-3 bg-blue-600/20 rounded-lg">
          <Paperclip class="w-5 h-5 text-blue-400" />
        </div>
        <div class="flex-1">
          <p class="text-white text-sm font-medium">{{ email.attachmentName }}</p>
          <p class="text-gray-500 text-xs">
            {{ email.attachmentCount }} attachment{{ email.attachmentCount > 1 ? 's' : '' }}
          </p>
        </div>
        <button class="px-4 py-2 text-sm text-blue-400 hover:text-blue-300 hover:bg-blue-600/10 rounded-lg transition-colors">
          Download
        </button>
      </div>
    </div>

    <!-- Signature -->
    <div class="text-gray-500 text-xs leading-5 mb-8 pb-6 border-b border-white/5">
      <p>This email was sent to {{ currentUser.email }}. If you have any questions, please reply directly to this email.</p>
    </div>

    <!-- Action Buttons -->
    <div class="flex gap-3 mt-auto">
      <button 
        @click="handleReply"
        class="flex items-center gap-2 px-6 py-2.5 bg-blue-600 hover:bg-blue-700 text-white rounded-lg transition-colors font-medium text-sm"
      >
        <Reply class="w-4 h-4" />
        Reply
      </button>
      <button class="flex items-center gap-2 px-6 py-2.5 bg-[#2B2B2B] hover:bg-[#363636] text-gray-300 rounded-lg transition-colors font-medium text-sm border border-white/5">
        <Forward class="w-4 h-4" />
        Forward
      </button>
    </div>
  </div>
</template>
