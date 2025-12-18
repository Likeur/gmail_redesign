<script setup>
import { ref, inject, computed } from 'vue';
import { Search, Bell, Settings, Link, X, Clock, Mail, User, Sparkles, PanelLeftClose, PanelLeft, ArrowLeft } from 'lucide-vue-next';
import Tooltip from './Tooltip.vue';

const currentUser = inject('currentUser');
const isSidebarCollapsed = inject('isSidebarCollapsed');
const toggleSidebar = inject('toggleSidebar');
const isMobile = inject('isMobile');
const showEmailList = inject('showEmailList');

// Notification dropdown state
const isNotificationsOpen = ref(false);

const notifications = ref([
  {
    id: 1,
    type: 'email',
    title: 'New email from Jeanne Nesty',
    message: 'UI Project: Client Dashboard',
    time: '2 min ago',
    unread: true
  },
  {
    id: 2,
    type: 'reminder',
    title: 'Meeting in 30 minutes',
    message: 'Q4 Review with Finance Team',
    time: '28 min ago',
    unread: true
  },
  {
    id: 3,
    type: 'update',
    title: 'Security update available',
    message: 'Review your account security settings',
    time: '1 hour ago',
    unread: false
  }
]);

const unreadCount = computed(() => notifications.value.filter(n => n.unread).length);

// Search overlay state
const isSearchOpen = ref(false);
const searchQuery = ref('');

const recentSearches = ref([
  'invoice 2024',
  'from:jeanne.nesty',
  'project dashboard',
  'has:attachment',
  'marketing campaign'
]);

const searchSuggestions = computed(() => {
  if (!searchQuery.value) return [];
  return [
    { type: 'filter', text: `from:${searchQuery.value}`, icon: 'user' },
    { type: 'filter', text: `to:${searchQuery.value}`, icon: 'mail' },
    { type: 'filter', text: `subject:${searchQuery.value}`, icon: 'mail' },
    { type: 'search', text: searchQuery.value, icon: 'search' },
    { type: 'filter', text: `has:attachment ${searchQuery.value}`, icon: 'sparkles' }
  ];
});

const openSearch = () => {
  isSearchOpen.value = true;
  searchQuery.value = '';
};

const closeSearch = () => {
  isSearchOpen.value = false;
  searchQuery.value = '';
};

const performSearch = (query) => {
  console.log('Searching for:', query);
  if (!recentSearches.value.includes(query)) {
    recentSearches.value.unshift(query);
    if (recentSearches.value.length > 5) recentSearches.value.pop();
  }
  closeSearch();
};

const goBackToList = () => {
  showEmailList.value = true;
};
</script>

<template>
  <header class="flex items-center justify-between px-3 md:px-4 py-3 bg-[#1E1E1E] text-white border-b border-white/10">
    <!-- Left: Toggle/Back + Context -->
    <div class="flex items-center gap-2 md:gap-4">
      <!-- Back Button (Mobile only, when viewing email detail) -->
      <button 
        v-if="isMobile && !showEmailList"
        @click="goBackToList"
        class="p-2 hover:bg-white/10 rounded-lg transition-colors text-gray-400 hover:text-white"
      >
        <ArrowLeft class="w-5 h-5" />
      </button>
      
      <!-- Sidebar Toggle Button (Hidden on mobile when viewing detail) -->
      <Tooltip v-if="!isMobile || showEmailList" :text="isSidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'" position="bottom">
        <button 
          @click="toggleSidebar"
          class="p-2 hover:bg-white/10 rounded-lg transition-colors text-gray-400 hover:text-white"
        >
          <PanelLeft v-if="isSidebarCollapsed" class="w-5 h-5" />
          <PanelLeftClose v-else class="w-5 h-5" />
        </button>
      </Tooltip>
      
      <div class="hidden sm:block">
        <h2 class="text-lg font-semibold">Inbox</h2>
        <p class="text-xs text-gray-500">2445 messages, 2 Unread</p>
      </div>
      <h2 v-if="isMobile" class="text-base font-semibold">{{ showEmailList ? 'Inbox' : 'Email' }}</h2>
    </div>

    <!-- Center: Search -->
    <div class="flex-1 max-w-2xl mx-8">
      <div class="relative group">
        <Search class="absolute left-3 top-1/2 -translate-y-1/2 w-5 h-5 text-gray-500 group-focus-within:text-blue-400 transition-colors" />
        <input 
          type="text" 
          placeholder="Search in mail"
          @click="openSearch"
          readonly
          class="w-full bg-[#2B2B2B] text-gray-200 rounded-lg pl-10 pr-4 py-2.5 focus:outline-none focus:ring-1 focus:ring-blue-500/50 transition-all placeholder:text-gray-600 cursor-pointer"
        >
      </div>
    </div>

    <!-- Right: Actions -->
    <div class="flex items-center gap-1">
      <Tooltip text="Connect" position="bottom">
        <button class="p-2 text-gray-400 hover:text-white hover:bg-white/10 rounded-lg transition-colors">
          <Link class="w-5 h-5" />
        </button>
      </Tooltip>
      
      <!-- Notifications Button with Dropdown -->
      <div class="relative">
        <Tooltip text="Notifications" position="bottom">
          <button 
            @click="isNotificationsOpen = !isNotificationsOpen"
            class="p-2 text-gray-400 hover:text-white hover:bg-white/10 rounded-lg transition-colors relative"
          >
            <Bell class="w-5 h-5" />
            <span v-if="unreadCount > 0" class="absolute top-1 right-1 w-4 h-4 bg-red-500 rounded-full text-[10px] font-bold flex items-center justify-center text-white">
              {{ unreadCount }}
            </span>
          </button>
        </Tooltip>
        
        <!-- Notifications Dropdown -->
        <Transition
          enter-active-class="transition ease-out duration-200"
          enter-from-class="opacity-0 scale-95 translate-y-1"
          enter-to-class="opacity-100 scale-100 translate-y-0"
          leave-active-class="transition ease-in duration-150"
          leave-from-class="opacity-100 scale-100 translate-y-0"
          leave-to-class="opacity-0 scale-95 translate-y-1"
        >
          <div 
            v-if="isNotificationsOpen" 
            class="absolute top-full right-0 mt-2 w-80 bg-[#2B2B2B] rounded-xl shadow-2xl border border-white/10 overflow-hidden z-50"
          >
            <div class="px-4 py-3 border-b border-white/5 flex items-center justify-between">
              <h3 class="text-white font-semibold">Notifications</h3>
              <button class="text-xs text-blue-400 hover:text-blue-300 transition-colors">Mark all as read</button>
            </div>
            
            <div class="max-h-80 overflow-y-auto">
              <div 
                v-for="notification in notifications" 
                :key="notification.id"
                class="px-4 py-3 hover:bg-white/5 transition-colors border-b border-white/5 last:border-0 cursor-pointer"
                :class="{ 'bg-blue-600/5': notification.unread }"
              >
                <div class="flex items-start gap-3">
                  <div 
                    class="w-8 h-8 rounded-full flex items-center justify-center shrink-0"
                    :class="{
                      'bg-blue-600/20 text-blue-400': notification.type === 'email',
                      'bg-orange-600/20 text-orange-400': notification.type === 'reminder',
                      'bg-green-600/20 text-green-400': notification.type === 'update'
                    }"
                  >
                    <Mail v-if="notification.type === 'email'" class="w-4 h-4" />
                    <Clock v-else-if="notification.type === 'reminder'" class="w-4 h-4" />
                    <Sparkles v-else class="w-4 h-4" />
                  </div>
                  <div class="flex-1 min-w-0">
                    <div class="flex items-start justify-between gap-2">
                      <p class="text-sm font-medium truncate" :class="notification.unread ? 'text-white' : 'text-gray-400'">
                        {{ notification.title }}
                      </p>
                      <span v-if="notification.unread" class="w-2 h-2 bg-blue-500 rounded-full shrink-0 mt-1.5"></span>
                    </div>
                    <p class="text-xs text-gray-500 truncate">{{ notification.message }}</p>
                    <p class="text-xs text-gray-600 mt-1">{{ notification.time }}</p>
                  </div>
                </div>
              </div>
            </div>
            
            <div class="px-4 py-3 border-t border-white/5 bg-[#252525]">
              <button class="w-full text-center text-sm text-blue-400 hover:text-blue-300 transition-colors">
                View all notifications
              </button>
            </div>
          </div>
        </Transition>
      </div>
      
      <Tooltip text="Settings" position="bottom">
        <button class="p-2 text-gray-400 hover:text-white hover:bg-white/10 rounded-lg transition-colors">
          <Settings class="w-5 h-5" />
        </button>
      </Tooltip>
      
      <img :src="currentUser.avatar" class="w-8 h-8 rounded-full border border-gray-600 ml-2" />
    </div>
  </header>

  <!-- Search Overlay -->
  <Teleport to="body">
    <Transition
      enter-active-class="transition ease-out duration-300"
      enter-from-class="opacity-0"
      enter-to-class="opacity-100"
      leave-active-class="transition ease-in duration-200"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div 
        v-if="isSearchOpen" 
        class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-start justify-center pt-20"
        @click.self="closeSearch"
      >
        <Transition
          enter-active-class="transition ease-out duration-300 delay-75"
          enter-from-class="opacity-0 scale-95 -translate-y-4"
          enter-to-class="opacity-100 scale-100 translate-y-0"
          leave-active-class="transition ease-in duration-200"
          leave-from-class="opacity-100 scale-100 translate-y-0"
          leave-to-class="opacity-0 scale-95 -translate-y-4"
        >
          <div 
            v-if="isSearchOpen"
            class="w-full max-w-2xl bg-[#1E1E1E] rounded-2xl shadow-2xl border border-white/10 overflow-hidden"
          >
            <!-- Search Input -->
            <div class="flex items-center gap-3 px-5 py-4 border-b border-white/5">
              <Search class="w-5 h-5 text-blue-400 shrink-0" />
              <input 
                v-model="searchQuery"
                type="text"
                placeholder="Search in emails..."
                class="flex-1 bg-transparent text-white text-lg focus:outline-none placeholder:text-gray-500"
                autofocus
                @keydown.enter="performSearch(searchQuery)"
                @keydown.esc="closeSearch"
              />
              <button 
                @click="closeSearch"
                class="p-1.5 hover:bg-white/10 rounded-lg transition-colors text-gray-400 hover:text-white"
              >
                <X class="w-5 h-5" />
              </button>
            </div>

            <!-- Search Content -->
            <div class="max-h-96 overflow-y-auto">
              <!-- Suggestions when typing -->
              <div v-if="searchQuery && searchSuggestions.length" class="p-2">
                <p class="px-3 py-2 text-xs text-gray-500 font-medium uppercase tracking-wider">Suggestions</p>
                <button 
                  v-for="(suggestion, index) in searchSuggestions" 
                  :key="index"
                  @click="performSearch(suggestion.text)"
                  class="w-full flex items-center gap-3 px-3 py-2.5 hover:bg-white/5 rounded-lg transition-colors text-left"
                >
                  <div class="w-8 h-8 rounded-lg bg-white/5 flex items-center justify-center">
                    <Search v-if="suggestion.icon === 'search'" class="w-4 h-4 text-gray-400" />
                    <User v-else-if="suggestion.icon === 'user'" class="w-4 h-4 text-blue-400" />
                    <Mail v-else-if="suggestion.icon === 'mail'" class="w-4 h-4 text-green-400" />
                    <Sparkles v-else class="w-4 h-4 text-purple-400" />
                  </div>
                  <span class="text-gray-300">{{ suggestion.text }}</span>
                  <span class="ml-auto text-xs text-gray-600">{{ suggestion.type }}</span>
                </button>
              </div>

              <!-- Recent searches when empty -->
              <div v-else class="p-2">
                <p class="px-3 py-2 text-xs text-gray-500 font-medium uppercase tracking-wider">Recent searches</p>
                <button 
                  v-for="(search, index) in recentSearches" 
                  :key="index"
                  @click="performSearch(search)"
                  class="w-full flex items-center gap-3 px-3 py-2.5 hover:bg-white/5 rounded-lg transition-colors text-left"
                >
                  <Clock class="w-4 h-4 text-gray-500" />
                  <span class="text-gray-300">{{ search }}</span>
                </button>
              </div>
            </div>

            <!-- Quick Filters -->
            <div class="px-4 py-3 border-t border-white/5 bg-[#1a1a1a]">
              <div class="flex items-center gap-2 flex-wrap">
                <span class="text-xs text-gray-500 mr-1">Quick filters:</span>
                <button 
                  v-for="filter in ['has:attachment', 'is:unread', 'is:starred', 'from:me']" 
                  :key="filter"
                  @click="performSearch(filter)"
                  class="px-3 py-1 text-xs bg-white/5 hover:bg-white/10 text-gray-400 hover:text-white rounded-full transition-colors"
                >
                  {{ filter }}
                </button>
              </div>
            </div>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>
