<script setup>
import { ref, inject } from 'vue';
import { 
  Inbox, 
  Send, 
  Star, 
  Archive, 
  Trash, 
  File, 
  Plus, 
  Folder,
  User,
  Settings,
  LogOut,
  Check,
  UserPlus
} from 'lucide-vue-next';
import Tooltip from './Tooltip.vue';

// Inject from App.vue
const currentUser = inject('currentUser');
const accounts = inject('accounts');
const switchAccount = inject('switchAccount');
const openCompose = inject('openCompose');
const isSidebarCollapsed = inject('isSidebarCollapsed');
const isMobile = inject('isMobile');

const isUserMenuOpen = ref(false);

const toggleUserMenu = () => {
  isUserMenuOpen.value = !isUserMenuOpen.value;
};

const handleSwitchAccount = (accountId) => {
  switchAccount(accountId);
  isUserMenuOpen.value = false;
};

const handleCompose = () => {
  openCompose();
};
</script>

<template>
  <aside 
    class="h-full flex flex-col bg-[#1E1E1E] text-gray-400 transition-all duration-300 ease-in-out border-r border-white/5"
    :class="[
      isSidebarCollapsed ? 'w-16 md:w-20 items-center py-4' : 'w-56 md:w-64 p-3 md:p-4',
      isMobile && isSidebarCollapsed ? 'w-0 overflow-hidden border-0 p-0' : ''
    ]"
  >
    <!-- Header: Logo -->
    <div class="mb-6 flex items-center justify-center" :class="[isSidebarCollapsed ? '' : 'px-2']">
      <!-- Gmail Icon - Always visible -->
      <div class="flex items-center gap-2 overflow-hidden">
        <svg class="w-8 h-6 shrink-0" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M24 5.457v13.909c0 .904-.732 1.636-1.636 1.636h-3.819V11.73L12 16.64l-6.545-4.91v9.273H1.636A1.636 1.636 0 0 1 0 19.366V5.457c0-2.023 2.309-3.178 3.927-1.964L5.455 4.64 12 9.548l6.545-4.91 1.528-1.145C21.69 2.28 24 3.434 24 5.457z" fill="#EA4335"/>
        </svg>
        <span v-if="!isSidebarCollapsed" class="text-xl font-medium text-white tracking-tight">Gmail</span>
      </div>
    </div>

    <!-- Compose Button -->
    <div class="mb-6 w-full" :class="[isSidebarCollapsed ? 'px-0 flex justify-center' : '']">
      <Tooltip v-if="isSidebarCollapsed" text="Compose" position="right">
        <button 
          @click="handleCompose"
          class="flex items-center gap-3 bg-[#C2E7FF] text-[#001D35] transition-all hover:bg-[#b0deff] hover:shadow-md p-4 rounded-full"
        >
          <Plus class="w-6 h-6" />
        </button>
      </Tooltip>
      <button 
        v-else
        @click="handleCompose"
        class="flex items-center gap-3 bg-[#C2E7FF] text-[#001D35] transition-all hover:bg-[#b0deff] hover:shadow-md px-6 py-4 rounded-2xl w-full"
      >
        <Plus class="w-6 h-6" />
        <span class="font-semibold text-sm">Compose</span>
      </button>
    </div>

    <!-- Navigation -->
    <nav class="flex-1 space-y-1 w-full overflow-y-auto scrollbar-hover" :class="[isSidebarCollapsed ? 'px-2' : '']">
      <!-- Inbox -->
      <Tooltip v-if="isSidebarCollapsed" text="Inbox (999+)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl bg-blue-600/20 text-blue-400">
          <Inbox class="w-5 h-5 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 rounded-r-full bg-blue-600/20 text-blue-400 border-l-4 border-blue-500">
        <Inbox class="w-5 h-5 shrink-0" />
        <span class="flex-1 truncate text-sm font-medium">Inbox</span>
        <span class="text-xs font-semibold">999+</span>
      </a>

      <!-- Sent -->
      <Tooltip v-if="isSidebarCollapsed" text="Sent (699)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <Send class="w-5 h-5 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors">
        <Send class="w-5 h-5 shrink-0" />
        <span class="flex-1 truncate text-sm">Sent</span>
        <span class="text-xs">699</span>
      </a>

      <!-- Starred -->
      <Tooltip v-if="isSidebarCollapsed" text="Starred (349)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <Star class="w-5 h-5 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors">
        <Star class="w-5 h-5 shrink-0" />
        <span class="flex-1 truncate text-sm">Starred</span>
        <span class="text-xs">349</span>
      </a>

      <!-- Archive -->
      <Tooltip v-if="isSidebarCollapsed" text="Archive (99)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <Archive class="w-5 h-5 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors">
        <Archive class="w-5 h-5 shrink-0" />
        <span class="flex-1 truncate text-sm">Archive</span>
        <span class="text-xs">99</span>
      </a>

      <!-- Delete -->
      <Tooltip v-if="isSidebarCollapsed" text="Delete (599)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <Trash class="w-5 h-5 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors">
        <Trash class="w-5 h-5 shrink-0" />
        <span class="flex-1 truncate text-sm">Delete</span>
        <span class="text-xs">599</span>
      </a>

      <!-- Draft -->
      <Tooltip v-if="isSidebarCollapsed" text="Draft (12)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <File class="w-5 h-5 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors">
        <File class="w-5 h-5 shrink-0" />
        <span class="flex-1 truncate text-sm">Draft</span>
        <span class="text-xs">12</span>
      </a>

      <!-- Folders Section -->
      <div class="pt-6 pb-2 px-4 text-xs font-semibold uppercase tracking-wider text-gray-500" v-if="!isSidebarCollapsed">
        Folder
        <button class="float-right hover:text-white"><Plus class="w-4 h-4" /></button>
      </div>
      <div v-else class="my-4 h-px bg-white/10 mx-4"></div>

      <!-- Finance -->
      <Tooltip v-if="isSidebarCollapsed" text="Finance (99)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <Folder class="w-5 h-5 text-blue-400 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors group">
        <Folder class="w-5 h-5 text-blue-400 group-hover:text-blue-300 shrink-0" />
        <span class="truncate">Finance</span>
        <span class="ml-auto text-xs">99</span>
      </a>

      <!-- Work -->
      <Tooltip v-if="isSidebarCollapsed" text="Work (69)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <Folder class="w-5 h-5 text-green-400 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors group">
        <Folder class="w-5 h-5 text-green-400 group-hover:text-green-300 shrink-0" />
        <span class="truncate">Work</span>
        <span class="ml-auto text-xs">69</span>
      </a>

      <!-- Personal -->
      <Tooltip v-if="isSidebarCollapsed" text="Personal (39)" position="right">
        <a href="#" class="flex items-center justify-center gap-4 px-4 py-3 rounded-xl hover:bg-white/5 transition-colors">
          <Folder class="w-5 h-5 text-purple-400 shrink-0" />
        </a>
      </Tooltip>
      <a v-else href="#" class="flex items-center gap-4 px-4 py-2 hover:bg-white/5 rounded-r-full transition-colors group">
        <Folder class="w-5 h-5 text-purple-400 group-hover:text-purple-300 shrink-0" />
        <span class="truncate">Personal</span>
        <span class="ml-auto text-xs">39</span>
      </a>
    </nav>

    <!-- User Profile & Dropdown -->
    <div class="mt-auto relative w-full" :class="[isSidebarCollapsed ? 'px-2' : 'px-0']">
       <!-- Dropdown Menu -->
        <Transition
          enter-active-class="transition-all duration-200 ease-out"
          enter-from-class="opacity-0 scale-95 translate-y-2"
          enter-to-class="opacity-100 scale-100 translate-y-0"
          leave-active-class="transition-all duration-150 ease-in"
          leave-from-class="opacity-100 scale-100 translate-y-0"
          leave-to-class="opacity-0 scale-95 translate-y-2"
        >
          <div 
            v-if="isUserMenuOpen" 
            class="absolute bg-[#2B2B2B] rounded-xl shadow-2xl border border-white/10 overflow-hidden z-20"
            :class="[isSidebarCollapsed ? 'bottom-0 left-full ml-2 w-64' : 'bottom-full left-0 w-full mb-2']"
          >
            <!-- Current Account Header -->
            <div class="p-4 border-b border-white/5">
              <div class="flex items-center gap-3">
                <img :src="currentUser.avatar" class="w-10 h-10 rounded-full ring-2 ring-blue-500" />
                <div class="flex-1 min-w-0">
                  <p class="text-white font-medium truncate">{{ currentUser.name }}</p>
                  <p class="text-xs text-gray-500 truncate">{{ currentUser.email }}</p>
                </div>
              </div>
            </div>

            <!-- Account Options -->
            <div class="p-2">
              <button class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-3 transition-colors rounded-lg">
                <User class="w-4 h-4" /> Manage your account
              </button>
            </div>

            <!-- Switch Account Section -->
            <div class="border-t border-white/5">
              <div class="px-4 py-2">
                <span class="text-xs text-gray-500 font-medium uppercase tracking-wider">Switch account</span>
              </div>
              <div class="px-2 pb-2 space-y-1 max-h-40 overflow-y-auto">
                <button 
                  v-for="account in accounts" 
                  :key="account.id"
                  @click="handleSwitchAccount(account.id)"
                  class="w-full text-left px-3 py-2 text-sm hover:bg-white/5 flex items-center gap-3 transition-colors rounded-lg"
                  :class="[account.id === currentUser.id ? 'text-blue-400' : 'text-gray-300 hover:text-white']"
                >
                  <img :src="account.avatar" class="w-8 h-8 rounded-full" :class="[account.id === currentUser.id ? 'ring-2 ring-blue-500' : '']" />
                  <div class="flex-1 min-w-0">
                    <p class="truncate font-medium">{{ account.name }}</p>
                    <p class="text-xs text-gray-500 truncate">{{ account.email }}</p>
                  </div>
                  <Check v-if="account.id === currentUser.id" class="w-4 h-4 text-blue-400 shrink-0" />
                </button>
                
                <!-- Add another account -->
                <button class="w-full text-left px-3 py-2 text-sm text-gray-300 hover:bg-white/5 hover:text-white flex items-center gap-3 transition-colors rounded-lg">
                  <div class="w-8 h-8 rounded-full bg-white/5 flex items-center justify-center">
                    <UserPlus class="w-4 h-4" />
                  </div>
                  <span>Add another account</span>
                </button>
              </div>
            </div>

            <!-- Sign Out -->
            <div class="border-t border-white/5 p-2">
              <button class="w-full text-left px-3 py-2 text-sm text-red-400 hover:bg-white/5 flex items-center gap-3 transition-colors rounded-lg">
                <LogOut class="w-4 h-4" /> Sign out of all accounts
              </button>
            </div>
          </div>
        </Transition>

      <Tooltip v-if="isSidebarCollapsed" :text="currentUser.name" position="right">
        <button 
          @click="toggleUserMenu"
          class="flex items-center justify-center p-2 hover:bg-white/5 transition-colors rounded-lg"
        >
          <img :src="currentUser.avatar" alt="User" class="w-8 h-8 rounded-full border border-gray-600 shrink-0" />
        </button>
      </Tooltip>
      <button 
        v-else
        @click="toggleUserMenu"
        class="flex items-center gap-3 w-full px-2 py-4 hover:bg-white/5 rounded-lg transition-colors text-left"
      >
        <img :src="currentUser.avatar" alt="User" class="w-8 h-8 rounded-full border border-gray-600 shrink-0" />
        <div class="flex-1 min-w-0">
           <p class="text-sm font-medium text-white truncate">{{ currentUser.name }}</p>
           <p class="text-xs text-gray-500 truncate">{{ currentUser.email }}</p>
        </div>
      </button>
    </div>
  </aside>
</template>
