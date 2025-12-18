<script setup>
import { ref, computed, provide } from 'vue';
import Sidebar from './components/Sidebar.vue'
import TopBar from './components/TopBar.vue'
import EmailList from './components/EmailList.vue'
import EmailDetail from './components/EmailDetail.vue'
import ComposeModal from './components/ComposeModal.vue'

// User accounts management
const accounts = ref([
  {
    id: 1,
    name: 'Antoine Piedanna',
    email: 'antoine@gmail.com',
    avatar: 'https://i.pravatar.cc/150?img=11'
  },
  {
    id: 2,
    name: 'Marie Dupont',
    email: 'marie.dupont@gmail.com',
    avatar: 'https://i.pravatar.cc/150?img=47'
  },
  {
    id: 3,
    name: 'Jean Martin',
    email: 'jean.martin@outlook.com',
    avatar: 'https://i.pravatar.cc/150?img=68'
  }
]);

const currentAccountId = ref(1);

const currentUser = computed(() => {
  return accounts.value.find(a => a.id === currentAccountId.value) || accounts.value[0];
});

const switchAccount = (accountId) => {
  currentAccountId.value = accountId;
};

// Sidebar collapsed state (managed here to share between TopBar and Sidebar)
const isSidebarCollapsed = ref(false);

// Mobile responsiveness
const isMobile = ref(false);
const showEmailList = ref(true); // On mobile: true = show list, false = show detail

const checkMobile = () => {
  isMobile.value = window.innerWidth < 768;
  // Auto-collapse sidebar on mobile
  if (isMobile.value) {
    isSidebarCollapsed.value = true;
  }
};

// Initialize and listen for resize
if (typeof window !== 'undefined') {
  checkMobile();
  window.addEventListener('resize', checkMobile);
}

const toggleSidebar = () => {
  isSidebarCollapsed.value = !isSidebarCollapsed.value;
};

// Provide user data to child components
provide('currentUser', currentUser);
provide('accounts', accounts);
provide('switchAccount', switchAccount);

// Provide sidebar state
provide('isSidebarCollapsed', isSidebarCollapsed);
provide('toggleSidebar', toggleSidebar);

// Provide mobile state
provide('isMobile', isMobile);
provide('showEmailList', showEmailList);

// Compose modal state
const isComposeOpen = ref(false);
const replyToEmail = ref(null);

const openCompose = () => {
  replyToEmail.value = null;
  isComposeOpen.value = true;
};

const openReply = (email) => {
  replyToEmail.value = email;
  isComposeOpen.value = true;
};

const closeCompose = () => {
  isComposeOpen.value = false;
  replyToEmail.value = null;
};

const handleSendEmail = (emailData) => {
  console.log('Sending email:', emailData);
  // In a real app, this would send the email via API
};

// Provide compose functions
provide('openCompose', openCompose);
provide('openReply', openReply);

// Centralized email data with real content
const emails = ref([
  {
    id: 1,
    sender: 'Jeanne Nesty',
    email: 'jeanne.nesty@company.com',
    subject: 'UI Project : Client Dashboard',
    preview: 'Hi Antoine, I wanted to share the latest updates on the dashboard project...',
    body: `Hi Antoine,

I wanted to share the latest updates on the dashboard project. The design team has finalized the new color scheme and typography guidelines. We've incorporated the feedback from the last review session and made significant improvements to the user flow.

Key updates include:
• Redesigned navigation with improved accessibility
• New data visualization components for better insights
• Optimized performance for faster load times
• Mobile-responsive layouts for all screen sizes

Please review the attached mockups and let me know your thoughts. We're aiming to have the development phase started by next week.

Looking forward to your feedback!

Best regards,
Jeanne`,
    time: '03:15 PM',
    date: '20 June 2024',
    avatar: 'https://i.pravatar.cc/150?img=32',
    hasAttachment: true,
    attachmentName: 'Dashboard.fig',
    attachmentCount: 3,
    image: 'https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=800&q=80',
    cc: 'lennon@gmail.com',
    folder: 'Finance'
  },
  {
    id: 2,
    sender: 'Mathias Goblet',
    email: 'mathias.goblet@finance.com',
    subject: 'Q4 Financial Report Review',
    preview: 'The quarterly financial report is ready for your review...',
    body: `Hello Antoine,

The quarterly financial report for Q4 is now ready for your review. This quarter showed strong performance across all key metrics.

Highlights:
• Revenue increased by 23% compared to Q3
• Customer acquisition cost decreased by 15%
• Net profit margin improved to 18.5%
• New market expansion contributed 12% to total revenue

I've scheduled a meeting for next Tuesday to discuss the findings in detail. Please review the attached spreadsheet before the meeting.

The board presentation is scheduled for the 28th, so we'll need final approval by the 25th.

Thanks,
Mathias`,
    time: '04:15 PM',
    date: '19 June 2024',
    avatar: 'https://i.pravatar.cc/150?img=12',
    hasAttachment: true,
    attachmentName: 'Q4_Report.xlsx',
    attachmentCount: 2,
    image: 'https://images.unsplash.com/photo-1460925895917-afdab827c52f?w=800&q=80',
    cc: '',
    folder: 'Finance'
  },
  {
    id: 3,
    sender: 'Spotify',
    email: 'notifications@spotify.com',
    subject: 'Your Daily Mix is ready!',
    preview: 'We\'ve curated a personalized playlist just for you based on your listening history...',
    body: `Hey there!

Your Daily Mix is ready and waiting for you. We've analyzed your recent listening patterns and created the perfect playlist for your day.

Today's mix includes:
• Fresh tracks from your favorite artists
• New releases we think you'll love
• Throwback hits you haven't heard in a while
• Curated picks based on your mood

Did you know? You've listened to over 500 hours of music this year! Your top genre is Electronic, followed by Indie and Alternative Rock.

Open Spotify now to start listening. Your music journey continues!

Happy listening,
The Spotify Team`,
    time: '05:15 PM',
    date: '19 June 2024',
    avatar: 'https://storage.googleapis.com/pr-newsroom-wp/1/2018/11/Spotify_Logo_RGB_Green.png',
    hasAttachment: false,
    attachmentName: '',
    attachmentCount: 0,
    image: 'https://images.unsplash.com/photo-1614680376593-902f74cf0d41?w=800&q=80',
    cc: '',
    folder: 'Personal'
  },
  {
    id: 4,
    sender: 'Apple',
    email: 'no-reply@apple.com',
    subject: 'Your Apple Invoice',
    preview: 'Thank you for your recent purchase. Here is your invoice...',
    body: `Dear Customer,

Thank you for your recent purchase from the App Store. Please find your invoice details below.

Order Details:
• iCloud+ 200GB - Monthly Subscription
• Apple Music - Family Plan
• App Store Purchase: Procreate

Total charged: $24.97

This amount has been charged to your payment method ending in 4532. If you have any questions about this invoice, please visit support.apple.com.

Thank you for choosing Apple.

Apple Support Team`,
    time: '12:15 PM',
    date: '18 June 2024',
    avatar: 'https://www.apple.com/ac/structured-data/images/knowledge_graph_logo.png',
    hasAttachment: true,
    attachmentName: 'Invoice.pdf',
    attachmentCount: 1,
    image: '',
    cc: '',
    folder: 'Personal'
  },
  {
    id: 5,
    sender: 'Alex Mika',
    email: 'alex.mika@marketing.io',
    subject: 'Marketing Campaign Results',
    preview: 'Great news! The latest campaign exceeded our expectations...',
    body: `Hi Antoine,

Great news! The latest marketing campaign exceeded all our expectations. Here are the key results:

Performance Metrics:
• Click-through rate: 4.8% (industry avg: 2.1%)
• Conversion rate: 12.3%
• Cost per acquisition: $8.50 (down 40% from last campaign)
• Total reach: 2.4 million impressions

The A/B testing showed that version B of the landing page performed 35% better than version A. I recommend we roll out this version across all channels.

Next steps:
1. Scale budget for top-performing channels
2. Create similar campaigns for Q1
3. Implement learnings across other product lines

Let's sync this week to plan the next phase.

Cheers,
Alex`,
    time: '01:15 PM',
    date: '18 June 2024',
    avatar: 'https://i.pravatar.cc/150?img=5',
    hasAttachment: true,
    attachmentName: 'Campaign_Report.pdf',
    attachmentCount: 4,
    image: 'https://images.unsplash.com/photo-1533750349088-cd871a92f312?w=800&q=80',
    cc: 'team@marketing.io',
    folder: 'Work'
  }
]);

const selectedEmailId = ref(1);

const selectedEmail = computed(() => {
  return emails.value.find(e => e.id === selectedEmailId.value) || emails.value[0];
});

const handleSelectEmail = (id) => {
  selectedEmailId.value = id;
};
</script>

<template>
  <div class="flex h-screen w-full bg-[#121212] text-white font-sans overflow-hidden">
    <!-- Left Sidebar - Hidden on mobile when viewing detail -->
    <div 
      class="flex-shrink-0 transition-all duration-300 ease-in-out"
      :class="{ 'hidden': isMobile && !showEmailList }"
    >
      <Sidebar />
    </div>

    <!-- Main Content Area -->
    <div class="flex-1 flex flex-col min-w-0">
      <TopBar />
      
      <!-- Split View: List + Detail -->
      <div class="flex-1 flex min-h-0 bg-[#1E1E1E] overflow-hidden">
        <!-- Email List - Full width on mobile, fixed on desktop -->
        <div 
          class="flex-shrink-0 border-r border-white/5 transition-all duration-300"
          :class="[
            isMobile 
              ? (showEmailList ? 'w-full' : 'hidden') 
              : 'w-80 lg:w-96'
          ]"
        >
           <EmailList 
             :emails="emails" 
             :selectedEmailId="selectedEmailId"
             @select="(id) => { handleSelectEmail(id); if (isMobile) showEmailList = false; }"
           />
        </div>
        
        <!-- Email Detail - Full width on mobile, flex on desktop -->
        <div 
          class="flex-1 min-w-0"
          :class="{ 'hidden': isMobile && showEmailList }"
        >
           <EmailDetail :email="selectedEmail" />
        </div>
      </div>
    </div>

    <!-- Compose Modal -->
    <ComposeModal 
      :isOpen="isComposeOpen"
      :replyTo="replyToEmail"
      :currentUser="currentUser"
      @close="closeCompose"
      @send="handleSendEmail"
    />
  </div>
</template>

<style>
body {
  font-family: 'Inter', sans-serif;
  background-color: #121212;
}
</style>
