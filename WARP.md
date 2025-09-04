# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Development Commands

### Core Development
```bash
# Start development server with hot reload
npm run dev

# Build for production
npm run build

# Preview production build locally
npm run preview

# Run ESLint for code quality checks
npm run lint
```

### Package Management
```bash
# Install dependencies
npm install

# Add new dependencies
npm install <package-name>

# Add development dependencies
npm install -D <package-name>
```

### Supabase Commands (if Supabase CLI is installed)
```bash
# Start Supabase local development
supabase start

# Generate types for database schema
supabase gen types typescript --local > src/types/database.ts

# Apply migrations
supabase db reset

# Deploy edge functions
supabase functions deploy optimize-resume
```

## Architecture Overview

### Technology Stack
- **Frontend**: React 19 + TypeScript + Vite
- **Styling**: Tailwind CSS
- **Authentication & Database**: Supabase
- **AI Integration**: GROQ SDK (via Supabase Edge Functions)
- **PDF Generation**: jsPDF, html2canvas
- **Document Processing**: docx, mammoth (for resume builder)
- **UI Components**: Custom components with Radix UI and Lucide icons
- **Animation**: Framer Motion
- **Notifications**: React Hot Toast
- **PWA**: Vite PWA plugin

### Project Structure
```
src/
├── components/          # Reusable UI components
│   ├── Navbar.tsx      # Main navigation with auth state
│   ├── ProtectedRoute.tsx  # Route protection wrapper
│   └── ConsultationModal.tsx
├── contexts/
│   └── AuthContext.tsx # Global auth state management
├── lib/
│   └── supabaseClient.ts  # Supabase client configuration
├── pages/              # Route components organized by feature
│   ├── auth/          # Login/signup pages
│   ├── admin/         # Admin dashboard and blog management
│   └── resume-builder/ # Multi-step resume builder
├── data/
│   └── blogData.ts    # Static blog data
└── utils/
    └── readTime.ts    # Blog reading time calculator
```

### Key Features & Architecture

#### Authentication Flow
- **Context**: `AuthContext` provides global auth state using Supabase Auth
- **Protection**: `ProtectedRoute` component handles route protection with role-based access (admin vs user)
- **Profile Management**: User profiles stored in Supabase `profiles` table with role field

#### Resume Builder (Multi-step AI-powered)
- **3-Step Process**: Create CV → AI Optimization → Download/Customize
- **AI Integration**: Uses GROQ SDK via Supabase Edge Function for job-specific resume optimization
- **Types**: Comprehensive TypeScript interfaces in `resume-builder/types.ts`
- **Templates**: Multiple resume templates (Modern, Classic) with customization options

#### Admin System
- **Role-based Access**: Admin users can access blog management features
- **Blog Management**: Create, edit, and manage blog posts through admin dashboard
- **Protected Routes**: Admin routes protected by `ProtectedRoute` with `adminOnly={true}`

#### AI-Powered Resume Optimization
- **Edge Function**: `supabase/functions/optimize-resume/index.ts` processes CV data with GROQ
- **Analysis Steps**: Progressive UI feedback during AI analysis
- **Suggestion Application**: Users can apply all suggestions or review individually

### Environment Variables

#### Required Variables (.env)
```
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

#### Supabase Edge Function Variables
```
GROQ_API_KEY=your_groq_api_key
```

### Database Schema
The project uses Supabase with the following key tables:
- `profiles` - User profiles with role-based access control
- Blog-related tables for content management
- Authentication handled by Supabase Auth

### Development Guidelines

#### TypeScript Usage
- Strict TypeScript configuration with comprehensive type checking
- All components use proper TypeScript interfaces
- Resume builder has detailed type definitions in `types.ts`

#### State Management
- React Context for global auth state
- Local component state for feature-specific data
- No external state management library (Redux, Zustand) currently used

#### Styling Approach
- Tailwind CSS for all styling
- Responsive design with mobile-first approach
- Custom configuration in `tailwind.config.js`
- Framer Motion for animations and transitions

#### API Integration
- Supabase client for database operations and auth
- Edge functions for AI/ML processing to avoid exposing API keys
- GROQ SDK integration for resume optimization

#### Error Handling
- Toast notifications for user feedback via React Hot Toast
- Try-catch blocks for async operations
- Graceful error handling in AI processing with timeout controls

#### Route Structure
- React Router with nested routes and protection
- Public routes: Home, About, Services, Portfolio, Blog, Contact
- Auth routes: Login, Signup
- Protected routes: Profile, Checkout
- Admin routes: Dashboard, Blog Management (admin-only)
- Utility routes: Invoice Generator, Resume Builder (public)

### Development Notes
- The codebase uses modern React patterns (hooks, functional components)
- Path alias configured for `@/` pointing to `src/` directory
- ESLint configured with React hooks and TypeScript rules
- No testing framework currently configured
- PWA features enabled through Vite PWA plugin
