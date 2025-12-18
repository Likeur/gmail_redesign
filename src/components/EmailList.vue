<script setup>
import { ref } from 'vue';
import { MoreVertical, Paperclip, ChevronDown, Trash, Archive, Star, Mail, Eye } from 'lucide-vue-next';

const props = defineProps({
  emails: {
    type: Array,
    required: true
  },
  selectedEmailId: {
    type: Number,
    required: true
  }
});

const emit = defineEmits(['select']);

// Filter dropdown state
const isFilterOpen = ref(false);
const selectedFilter = ref('Primary');
const filterOptions = ['Primary', 'Social', 'Promotions', 'Updates', 'Forums'];

const selectFilter = (filter) => {
  selectedFilter.value = filter;
  isFilterOpen.value = false;
};

// Action menu state (per email)
const openActionMenuId = ref(null);

const toggleActionMenu = (emailId, event) => {
  event.stopPropagation();
  openActionMenuId.value = openActionMenuId.value === emailId ? null : emailId;
};

const closeActionMenu = () => {
  openActionMenuId.value = null;
};

const handleAction = (action, emailId, event) => {
  event.stopPropagation();
  console.log(`Action: ${action} on email ${emailId}`);
  openActionMenuId.value = null;
};

// Group emails by date (simplified)
const todayEmails = props.emails.filter((_, i) => i < 3);
const yesterdayEmails = props.emails.filter((_, i) => i >= 3);
</script>

<template>
  <div class="h-full bg-[#1E1E1E] flex flex-col" @click="closeActionMenu">
    <!-- Header with Filter -->
    <div class="px-6 py-4 flex items-center justify-between border-b border-white/5">
      <!-- Custom Filter Dropdown -->
      <div class="relative">
        <button 
          @click="isFilterOpen = !isFilterOpen"
          class="flex items-center gap-2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg text-sm font-medium transition-colors"
        >
          {{ selectedFilter }}
          <ChevronDown class="w-4 h-4 transition-transform" :class="{ 'rotate-180': isFilterOpen }" />
        </button>
        
        <!-- Filter Dropdown Menu -->
        <div 
          v-if="isFilterOpen" 
          class="absolute top-full left-0 mt-2 w-48 bg-[#2B2B2B] rounded-lg shadow-xl border border-white/10 overflow-hidden z-30"
        >
          <button 
            v-for="filter in filterOptions" 
            :key="filter"
            @click="selectFilter(filter)"
            class="w-full text-left px-4 py-2.5 text-sm transition-colors flex items-center justify-between"
            :class="[
              selectedFilter === filter 
                ? 'bg-blue-600/20 text-blue-400' 
                : 'text-gray-300 hover:bg-white/5 hover:text-white'
            ]"
          >
            {{ filter }}
            <span v-if="selectedFilter === filter" class="text-blue-400">✓</span>
          </button>
        </div>
      </div>

      <!-- Global Actions -->
      <button class="p-2 text-gray-400 hover:text-white hover:bg-white/5 rounded-lg transition-colors">
        <MoreVertical class="w-5 h-5"/>
      </button>
    </div>

    <!-- Email List -->
    <div class="overflow-y-auto flex-1 px-4 py-4 space-y-6">
      <!-- Today Section -->
      <div v-if="todayEmails.length">
        <h3 class="text-gray-500 text-xs font-semibold uppercase tracking-wider mb-3 pl-2">Today</h3>
        <div class="space-y-2">
          <div 
            v-for="email in todayEmails" 
            :key="email.id" 
            @click="emit('select', email.id)"
            :class="[
              'p-4 rounded-xl cursor-pointer transition-all border-l-4 relative group',
              email.id === selectedEmailId 
                ? 'bg-[#2B2B2B] border-blue-500' 
                : 'hover:bg-[#2B2B2B]/50 border-transparent'
            ]"
          >
            <div class="flex items-start gap-3">
              <img :src="email.avatar" class="w-10 h-10 rounded-full object-cover" />
              <div class="flex-1 min-w-0">
                <div class="flex justify-between items-start mb-1">
                  <h4 class="text-white font-medium truncate">{{ email.sender }}</h4>
                  <div class="flex items-center gap-2">
                    <span class="text-xs text-gray-500 whitespace-nowrap">{{ email.time }}</span>
                    <!-- Action Menu Button -->
                    <div class="relative">
                      <button 
                        @click="toggleActionMenu(email.id, $event)"
                        class="p-1 text-gray-500 hover:text-white hover:bg-white/10 rounded opacity-0 group-hover:opacity-100 transition-all"
                      >
                        <MoreVertical class="w-4 h-4" />
                      </button>
                      
                      <!-- Action Dropdown -->
                      <div 
                        v-if="openActionMenuId === email.id" 
                        class="absolute top-full right-0 mt-1 w-40 bg-[#2B2B2B] rounded-lg shadow-xl border border-white/10 overflow-hidden z-40"
                      >
                        <button 
                          @click="handleAction('archive', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-2 transition-colors"
                        >
                          <Archive class="w-4 h-4" /> Archive
                        </button>
                        <button 
                          @click="handleAction('star', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-2 transition-colors"
                        >
                          <Star class="w-4 h-4" /> Star
                        </button>
                        <button 
                          @click="handleAction('markUnread', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-2 transition-colors"
                        >
                          <Eye class="w-4 h-4" /> Mark as unread
                        </button>
                        <div class="h-px bg-white/10 my-1"></div>
                        <button 
                          @click="handleAction('delete', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-red-400 hover:bg-white/5 flex items-center gap-2 transition-colors"
                        >
                          <Trash class="w-4 h-4" /> Delete
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
                <p class="text-gray-400 text-sm truncate mb-1">{{ email.subject }}</p>
                <p class="text-gray-500 text-xs line-clamp-2 leading-relaxed">{{ email.preview }}</p>
                
                <!-- Attachments -->
                <div v-if="email.hasAttachment" class="mt-3 flex gap-2 flex-wrap">
                  <div class="flex items-center gap-2 bg-[#1E1E1E] border border-white/10 rounded px-2 py-1 text-xs text-gray-300">
                    <Paperclip class="w-3 h-3 text-gray-500" />
                    <span class="truncate max-w-[100px]">{{ email.attachmentName }}</span>
                  </div>
                  <div v-if="email.attachmentCount > 1" class="flex items-center justify-center bg-[#1E1E1E] border border-white/10 rounded px-2 text-xs text-gray-400">
                    +{{ email.attachmentCount - 1 }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Yesterday Section -->
      <div v-if="yesterdayEmails.length">
        <h3 class="text-gray-500 text-xs font-semibold uppercase tracking-wider mb-3 pl-2">Yesterday</h3>
        <div class="space-y-2">
          <div 
            v-for="email in yesterdayEmails" 
            :key="email.id" 
            @click="emit('select', email.id)"
            :class="[
              'p-4 rounded-xl cursor-pointer transition-all border-l-4 relative group',
              email.id === selectedEmailId 
                ? 'bg-[#2B2B2B] border-blue-500' 
                : 'hover:bg-[#2B2B2B]/50 border-transparent'
            ]"
          >
            <div class="flex items-start gap-3">
              <img :src="email.avatar" class="w-10 h-10 rounded-full object-cover" />
              <div class="flex-1 min-w-0">
                <div class="flex justify-between items-start mb-1">
                  <h4 class="text-white font-medium truncate">{{ email.sender }}</h4>
                  <div class="flex items-center gap-2">
                    <span class="text-xs text-gray-500 whitespace-nowrap">{{ email.time }}</span>
                    <div class="relative">
                      <button 
                        @click="toggleActionMenu(email.id, $event)"
                        class="p-1 text-gray-500 hover:text-white hover:bg-white/10 rounded opacity-0 group-hover:opacity-100 transition-all"
                      >
                        <MoreVertical class="w-4 h-4" />
                      </button>
                      
                      <div 
                        v-if="openActionMenuId === email.id" 
                        class="absolute top-full right-0 mt-1 w-40 bg-[#2B2B2B] rounded-lg shadow-xl border border-white/10 overflow-hidden z-40"
                      >
                        <button 
                          @click="handleAction('archive', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-2 transition-colors"
                        >
                          <Archive class="w-4 h-4" /> Archive
                        </button>
                        <button 
                          @click="handleAction('star', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-2 transition-colors"
                        >
                          <Star class="w-4 h-4" /> Star
                        </button>
                        <button 
                          @click="handleAction('markUnread', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-2 transition-colors"
                        >
                          <Eye class="w-4 h-4" /> Mark as unread
                        </button>
                        <div class="h-px bg-white/10 my-1"></div>
                        <button 
                          @click="handleAction('delete', email.id, $event)"
                          class="w-full text-left px-3 py-2 text-sm text-red-400 hover:bg-white/5 flex items-center gap-2 transition-colors"
                        >
                          <Trash class="w-4 h-4" /> Delete
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
                <p class="text-gray-400 text-sm truncate mb-1">{{ email.subject }}</p>
                <p class="text-gray-500 text-xs line-clamp-2 leading-relaxed">{{ email.preview }}</p>
                
                <div v-if="email.hasAttachment" class="mt-3 flex gap-2 flex-wrap">
                  <div class="flex items-center gap-2 bg-[#1E1E1E] border border-white/10 rounded px-2 py-1 text-xs text-gray-300">
                    <Paperclip class="w-3 h-3 text-gray-500" />
                    <span class="truncate max-w-[100px]">{{ email.attachmentName }}</span>
                  </div>
                  <div v-if="email.attachmentCount > 1" class="flex items-center justify-center bg-[#1E1E1E] border border-white/10 rounded px-2 text-xs text-gray-400">
                    +{{ email.attachmentCount - 1 }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
