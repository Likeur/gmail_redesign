# 📧 Gmail Redesign

A modern, sleek redesign of Gmail built with **Vue.js 3** and **Tailwind CSS v4**. This project showcases a dark-themed, responsive email client with smooth animations and a premium user experience.

<img width="1920" height="1080" alt="gmail redesign" src="https://github.com/user-attachments/assets/34e94710-e688-40b4-8852-e49e37befd6c" />


## ✨ Features

### 🎨 Modern Dark Theme
- Elegant dark UI with carefully crafted color palette
- Smooth gradients and subtle borders
- Custom scrollbars matching the theme
- Premium glassmorphism effects

### 📱 Fully Responsive
- **Desktop**: Side-by-side email list and detail view with collapsible sidebar
- **Mobile**: Full-screen list/detail views with intuitive back navigation
- Automatic layout adaptation based on screen size
- Touch-friendly interface

### 📬 Email Management
- **Email List**: Browse emails with sender avatars, subjects, and previews
- **Email Detail**: Full email view with images, attachments, and rich formatting
- **Reply & Forward**: Quick actions with pre-filled compose modal
- **Folder Organization**: Finance, Work, Personal folders with color coding

### ✏️ Compose Email
- Floating compose modal with minimize/maximize states
- Smooth morph animations between states
- Pre-fill support for replies
- Attachment toolbar (Paperclip, Image, Link, Emoji)
- Full-screen mode on mobile

### 👤 Multi-Account Support
- Switch between multiple connected accounts
- Dynamic avatar updates across the UI
- "Add another account" functionality
- Account management dropdown

### 🔔 Notifications
- Real-time notification dropdown
- Unread count badge
- Different notification types (email, reminder, update)
- "Mark all as read" functionality

### 🔍 Advanced Search
- Full-screen search overlay with backdrop blur
- Recent searches history
- Live search suggestions
- Quick filter buttons (has:attachment, is:unread, is:starred, from:me)

### 💫 Animations & Interactions
- Custom tooltips with arrow pointers
- Smooth sidebar collapse/expand transitions
- Compose modal morph animations
- Dropdown enter/leave transitions
- Hover effects on interactive elements

## 🛠️ Tech Stack

- **Framework**: [Vue.js 3](https://vuejs.org/) with Composition API
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/)
- **Icons**: [Lucide Vue Next](https://lucide.dev/)
- **Build Tool**: [Vite](https://vitejs.dev/)
- **Language**: JavaScript

## 📦 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Likeur/gmail_redesign.git
   cd gmail_redesign
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm run dev
   ```

4. **Open in browser**
   ```
   http://localhost:5173
   ```

## 🏗️ Project Structure

```
gmail/
├── src/
│   ├── components/
│   │   ├── ComposeModal.vue    # Email composition modal
│   │   ├── EmailDetail.vue     # Email content display
│   │   ├── EmailList.vue       # Email list with filters
│   │   ├── Sidebar.vue         # Navigation sidebar
│   │   ├── Tooltip.vue         # Custom tooltip component
│   │   └── TopBar.vue          # Header with search & actions
│   ├── App.vue                 # Main application layout
│   ├── main.js                 # Vue app entry point
│   └── style.css               # Global styles & custom scrollbar
├── index.html
├── package.json
├── tailwind.config.js
└── vite.config.js
```

## 🎯 Key Components

### Sidebar (`Sidebar.vue`)
- Collapsible navigation with smooth transitions
- Gmail logo and branding
- Navigation items with unread counts
- Folder organization with color-coded icons
- User profile with account switching dropdown
- Custom tooltips when collapsed

### TopBar (`TopBar.vue`)
- Sidebar toggle button
- Inbox title with message count
- Search input triggering overlay
- Notification bell with dropdown
- Settings and user avatar

### EmailList (`EmailList.vue`)
- Scrollable email items
- Custom filter dropdown (Primary, Social, Promotions, etc.)
- Per-email action menu (Archive, Star, Mark as unread, Delete)
- Selection highlighting
- Real email data with avatars

### EmailDetail (`EmailDetail.vue`)
- Email header with subject and folder tag
- Sender info with avatar
- Full email body with paragraph support
- Inline images display
- Attachment cards with download
- Reply/Forward action buttons
- Custom tooltips on toolbar icons

### ComposeModal (`ComposeModal.vue`)
- Teleported to body for proper z-index
- Minimize/Maximize/Close controls
- From/To/Subject/Body fields
- Attachment toolbar
- Send button with validation
- Full-screen on mobile
- Smooth morph animations

### Tooltip (`Tooltip.vue`)
- Teleported to body (avoids overflow:hidden issues)
- Dynamic positioning (top, right, bottom, left)
- Arrow pointer
- Smooth enter/leave animations
- Configurable delay

## 🎨 Design Decisions

### Color Palette
- **Background**: `#121212` (main), `#1E1E1E` (cards), `#2B2B2B` (elevated)
- **Primary**: Blue-400 for active states and CTAs
- **Accent**: Red for delete, Green for success
- **Text**: White, Gray-300, Gray-500, Gray-600

### Typography
- Font: Inter (Google Fonts)
- Headings: Bold, larger sizes
- Body: Regular weight, comfortable line height

### Spacing
- Consistent padding: 4, 6, 8 units
- Gap utilities for flexbox layouts
- Responsive adjustments (p-4 md:p-6)

## 📱 Responsive Breakpoints

| Breakpoint | Width | Behavior |
|------------|-------|----------|
| Mobile | < 768px | Full-screen views, sidebar hidden, back navigation |
| Tablet | 768px - 1024px | Compact sidebar, narrower email list |
| Desktop | > 1024px | Full sidebar, side-by-side layout |

## 🚀 Build for Production

```bash
npm run build
```

The built files will be in the `dist/` directory.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Likeur (Jack Simba)** - [GitHub](https://github.com/Likeur)

---

<p align="center">
  Made with ❤️ using Vue.js and Tailwind CSS
</p>
