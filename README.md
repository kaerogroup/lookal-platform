# LookaL Platform

## Project Overview
LookaL adalah self-service phygital advertising platform yang menggabungkan physical digital signage network dengan online campaign management system. Platform ini membolehkan SME businesses create dan deploy advertising campaigns ke network 50,000 Android TV nodes across Malaysia.

## Core Architecture
- **Frontend**: React dashboard untuk advertisers dan publishers
- **Backend**: Vercel serverless functions + Supabase PostgreSQL database
- **Node Network**: Android TV apps running WebView untuk dynamic content rendering
- **Content Delivery**: Metadata-driven templates instead of static image generation

## Key Technical Components

### Campaign Management System
- Framework-driven advertising creation (AIDA, PAS, FOMO, etc.)
- Dynamic poster generation using HTML/CSS templates
- Real-time content updates without asset regeneration
- Typography dan styling stored as database metadata

### Geographic Distribution Algorithm
- Automated node assignment based on state/district/mukim selection
- Industry conflict avoidance (F&B ads don't show dalam F&B premises)
- Fair revenue distribution across publisher network
- Load balancing untuk advertising inventory

### Revenue Sharing Model
- Publishers receive 20% dari RM10/day advertising slots
- Automated payment calculation dan distribution
- Dashboard transparency untuk earnings tracking
- Bulk insurance management untuk node hardware

### Phygital Integration
- QR code generation untuk offline-to-online conversion
- Analytics tracking untuk scan rates dan engagement
- POS system integration untuk transaction processing
- SuperApp development untuk ecosystem completion

## Database Schema Highlights
- campaigns table: Content, styling, dan framework data
- nodes table: Publisher locations dengan geographic coordinates
- ad_slots table: Scheduling dan inventory management
- analytics_events table: Performance tracking across network

## Performance Requirements
- Support 1.5M daily ad impressions across network
- Real-time content distribution dengan <3 second load times
- Offline capability melalui local caching
- Bandwidth optimization: metadata updates vs full asset downloads

## Business Logic
- Framework psychology untuk marketing effectiveness
- Word limit enforcement untuk optimal readability
- Industry-specific video assignment untuk Zone B content
- Anti-abuse systems untuk QR scan validation

---

LookaL platform creates Malaysia's first self-service local advertising network yang bridges physical locations dengan digital engagement, enabling SMEs to access professional advertising capabilities at affordable pricing while providing publishers passive income opportunities through hardware investment.
