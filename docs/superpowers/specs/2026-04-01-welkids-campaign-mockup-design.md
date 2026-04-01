# WelKids AD3 E K2 Campaign — Mockup Design Spec

## Overview

Interactive mockup website for the WelKids AD3 E K2 campaign — a children's vitamin supplement product. The site has 6 modules showcasing the campaign's activities. Purpose: present to client as a visual prototype so they understand layout, flow, and what content they need to provide.

**Type**: Mockup + basic interactivity (HTML/CSS/JS with routing, hover, click interactions)
**Framework**: Astro (static site generator) — shared layouts/components, static HTML output
**Language**: Vietnamese throughout
**Content**: Placeholder/mock data — designed so client knows what to fill in

---

## Design System

### Philosophy

- Friendly, not childish — rounded/soft but with geometric structure
- Pastel with breathing room — 60-30-10 color rule
- Subtle depth — Flat 2.0 with glass effects, no old-school 3D

### Fonts

| Role | Font | Weights |
|------|------|---------|
| Headings & Display | Montserrat Alternates | 400, 500, 600, 700, 800 |
| Body, UI, Captions | Quicksand | 400, 500, 600, 700 |

Both fonts have full Vietnamese (UTF-8) support.

**Type Scale:**

| Token | Font | Size | Weight |
|-------|------|------|--------|
| H1 | Montserrat Alternates | 36px | 700 |
| H2 | Montserrat Alternates | 26px | 600 |
| H3 | Quicksand | 20px | 700 |
| Body | Quicksand | 16px | 500 |
| Small | Quicksand | 14px | 500 |
| Caption | Quicksand | 12px | 500 |

### Color Palette

**60% Neutral (backgrounds, breathing room):**

| Name | Hex | Usage |
|------|-----|-------|
| Kem trắng | `#FAFDF9` | Page background |
| Xám mint | `#F0F5F1` | Section backgrounds |
| Xám lá | `#E3ECE5` | Dividers, borders |
| Trắng sữa | `#FFFFFF` | Card backgrounds (non-glass) |

**30% Primary (brand identity):**

| Name | Hex | Usage |
|------|-----|-------|
| Primary Dark | `#3D8B4F` | Button shadows, active states |
| Primary | `#5EAA6E` | Buttons, links, accents |
| Primary Light | `#7DC08A` | Hover states, secondary |
| Primary Tint | `#D4EDDA` | Light backgrounds, tags |

**10% Accent (CTA, highlights):**

| Name | Hex | Usage |
|------|-----|-------|
| Accent Dark | `#E8AA20` | Button shadows |
| Accent | `#F5C842` | CTA buttons, badges |
| Accent Light | `#F8D76E` | Hover states |
| Accent Tint | `#FFF8E1` | Highlight backgrounds |

**Text Colors:**

| Name | Hex | Usage |
|------|-----|-------|
| Heading | `#2D4A35` | H1-H2, emphasis |
| Body | `#5A7D66` | Body text, descriptions |
| Muted | `#9AB5A3` | Captions, placeholders |

**Semantic:**

| Name | Hex | Usage |
|------|-----|-------|
| Error | `#E57373` | Error states |
| Warning | Uses Accent palette | Warning states |
| Success | Uses Primary palette | Success states |

### Glass Effects

Three glass variants replace traditional solid cards. Each uses `backdrop-filter: blur()` with gradient pseudo-element borders.

**Leaf Glass** — Primary tone, for cards, panels, navbar:
```css
.leaf-glass {
  background: rgba(255, 255, 255, 0.45);
  backdrop-filter: blur(16px);
  box-shadow: inset 0 1px 2px rgba(255,255,255,0.6), 0 4px 24px rgba(45,74,53,0.08);
}
/* ::before gradient border: white top → transparent middle → green-tinted bottom */
```

**Warm Glass** — Accent tone, for CTA areas, featured items:
```css
.warm-glass {
  background: rgba(255, 248, 225, 0.5);
  backdrop-filter: blur(16px);
  box-shadow: inset 0 1px 2px rgba(255,255,255,0.5), 0 4px 24px rgba(213,165,32,0.08);
}
/* ::before gradient border: white top → transparent middle → yellow-tinted bottom */
```

**Frost Glass** — Neutral, for inputs, badges, tooltips:
```css
.frost-glass {
  background: rgba(255, 255, 255, 0.35);
  backdrop-filter: blur(12px);
  box-shadow: inset 0 1px 1px rgba(255,255,255,0.5), 0 2px 16px rgba(0,0,0,0.04);
}
/* ::before gradient border: white top → transparent bottom */
```

### Components

**Buttons** — Pill-shaped (`border-radius: 50px`), gradient top-to-bottom, solid bottom-shadow for press-feel:

| Variant | Background | Text | Shadow |
|---------|-----------|------|--------|
| Primary | `#6DB87A → #5EAA6E → #4F9C60` | `#fff` | `0 2px 0 #3D8B4F` |
| Accent | `#F8D76E → #F5C842 → #E8B830` | `#6B5010` | `0 2px 0 #D4A520` |
| Warm | `#FFCC80 → #FFB74D → #FFA726` | `#6B4010` | `0 2px 0 #E69520` |
| Secondary | `#fff` border `#C8E0CC` | `#5EAA6E` | `0 2px 0 #C8E0CC` |
| Ghost | `#F0F5F1` | `#5A7D66` | `0 2px 0 #DCE5DD` |
| Glass | `rgba(255,255,255,0.6)` + blur | `#5A7D66` | inset highlight |

**Cards** — Glass style on gradient backgrounds:
- `border-radius: 24px`
- Floating icon in glass box (animation: gentle float 4s)
- Badge in frost-glass pill (top-right)
- Decorative radial gradient blobs in background

**Form Elements:**
- Inputs: `border-radius: 12px`, border `#E3ECE5`, bg `#FAFDF9`
- Choice cards: frost-glass, `border-radius: 16px`, emoji + label, selected state with green ring
- Progress bar: rounded, gradient green fill

**Shadows & Elevation:**

| Level | Usage | Value |
|-------|-------|-------|
| 1 | Flat elements | `0 1px 3px rgba(45,74,53,0.06)` |
| 2 | Cards, buttons | `0 2px 0 border-color, 0 4px 12px rgba(45,74,53,0.06)` |
| 3 | Modals, dropdowns | `0 4px 0 border-color, 0 8px 24px rgba(45,74,53,0.08)` |
| 4 | Hero, floating | `0 8px 0 border-color, 0 16px 40px rgba(45,74,53,0.1)` |

**Spacing:**

| Token | Value |
|-------|-------|
| micro | 4px |
| tight | 8px |
| base | 16px |
| comfortable | 24px |
| spacious | 32px |
| section | 48px |
| page-section | 64-80px |

**Layout:** Max-width 1200px, 12-column grid, gap 16-24px, padding 24px (mobile: 16px)

**Border Radius:**

| Element | Radius |
|---------|--------|
| Inputs | 12px |
| Icons | 16px |
| Cards | 20-24px |
| Buttons/pills | 50px |
| Avatars | 50% |

### Decorative Elements

- **Abstract blobs**: Radial gradients (green/yellow) as background decoration
- **Floating animation**: Gentle vertical float (4s ease-in-out infinite) on icons
- **Gradient backgrounds**: Multi-stop gradients using palette colors as section backdrops

### Animations (for Astro implementation)

- **Fade-up on scroll**: Elements enter with `opacity: 0 → 1` and `translateY(20px → 0)`, staggered delays
- **Float**: Icons gently float up/down (CSS keyframe)
- **Hover states**: Buttons scale slightly, cards lift with increased shadow

---

## Module 1: Trang Chủ & Giới Thiệu (Homepage)

### Purpose
Landing page introducing the WelKids AD3 E K2 campaign. First impression — showcase product, campaign activities, and guide users to each module.

### Sections (top to bottom)

**1.1 Navbar** (fixed, glass pill)
- Left: Logo (dinosaur mascot icon in green gradient box) + "WelKids" text
- Center: Nav links — Trang chủ, Phòng khám, Lớp học, Thực hành, Giải thưởng
- Right: "Liên hệ" primary button
- Style: leaf-glass, `border-radius: 50px`, fixed top, z-50
- Mobile: hamburger menu

**1.2 Hero Section** (full viewport or near-full)
- Background: Gradient green-to-yellow with abstract blob decorations
- Content (centered):
  - Frost-glass badge: "Campaign 2026" with green dot
  - H1: "Bộ Tứ Vàng WelKids AD₃ E K₂" (Montserrat Alternates)
  - Subtitle: "Vững nền miễn dịch · Bé khỏe vươn cao"
  - Two CTAs: "Khám phá ngay" (white glass button) + "Xem video" (transparent glass button)
- Placeholder: space for product image / banner image overlay

**1.3 Giới thiệu sản phẩm** (Product intro)
- Split layout: Image placeholder (left) + text content (right)
- Content: Product name, key vitamins (A, D3, E, K2), brief description
- 4 vitamin highlight cards (leaf-glass, icon + name + benefit)
- Placeholder text client needs to fill

**1.4 Giới thiệu Campaign** (Campaign overview)
- H2: "Hành trình khám phá cùng WelKids"
- 5 module cards (glass style) linking to each section:
  - Phòng khám (Health Check)
  - Lớp học (Video Learning)
  - Thực hành (AI Image)
  - Giải thưởng (Awards)
  - Each card: icon + title + short description + CTA button
- Laid out in a grid, glass cards on gradient background

**1.5 Form liên hệ** (Contact form)
- Leaf-glass panel
- Fields: Họ tên, Email, Số điện thoại, Tin nhắn
- Primary submit button
- Placeholder labels indicate what data client collects

**1.6 Footer** (full-width, no border-radius)
- Background: `#2D4A35` (dark green)
- 4 columns: Brand info, Khám phá links, Hoạt động links, Social icons
- Bottom bar: Copyright + Điều khoản / Chính sách
- Subtle radial glow decoration

---

## Module 2: Phòng Khám — Health Check Tool

### Purpose
Questionnaire-style health check. User answers questions about their child, gets nutrition assessment results with charts, product recommendations, and text conclusions.

### Flow
QR code scan → Landing page → Start questionnaire → Answer 5 questions → View results

### Pages

**2.1 Landing / Intro**
- Brief explanation of the health check
- "Bắt đầu kiểm tra" CTA button
- Estimated time: "Chỉ mất 2 phút"
- Leaf-glass container on gradient background

**2.2 Questionnaire (5 steps)**
- Progress bar at top (green gradient, shows step X/5)
- Leaf-glass main panel containing:
  - Question title (H2, Montserrat Alternates)
  - Subtitle/context
  - Answer options as choice cards (frost-glass, 2x2 grid)
  - Each choice: emoji + label, selected state = green ring + checkmark
  - Navigation: "Quay lại" ghost button + "Tiếp tục" primary button

**Mock questions (placeholder content):**
1. Bé bao nhiêu tuổi? (0-6 tháng / 6-12 tháng / 1-3 tuổi / 3-6 tuổi)
2. Chế độ ăn của bé? (Rau củ đa dạng / Cơm cháo chủ yếu / Sữa là chính / Thịt cá đầy đủ)
3. Bé có hay ốm vặt không? (Rất ít / Thỉnh thoảng / Thường xuyên / Rất thường xuyên)
4. Bé có tiếp xúc ánh nắng thường xuyên? (Hàng ngày / 2-3 lần/tuần / Hiếm khi / Không)
5. Bé có đang bổ sung vitamin? (Có, đầy đủ / Có, không đều / Chưa bổ sung / Không biết)

**2.3 Results Page**
Three sections in leaf-glass panels:

**a) Biểu đồ dinh dưỡng** (Nutrition chart)
- Radar/spider chart or bar chart showing nutritional assessment
- 4 axes: Vitamin A, D3, E, K2
- Mock data with color coding (green = good, yellow = cần cải thiện, red = thiếu)
- Chart placeholder — actual chart library in implementation

**b) Kết luận** (Text conclusion)
- Summary text based on answers
- Key findings highlighted in accent boxes
- Overall score/status badge

**c) Đề xuất sản phẩm** (Product recommendation)
- WelKids AD3 E K2 product card
- Product image placeholder
- Key benefits listed
- "Tìm hiểu thêm" / "Mua ngay" CTA buttons
- Dosage recommendation based on age

**d) Share / Download**
- "Chia sẻ kết quả" button
- "Làm lại bài kiểm tra" ghost button

---

## Module 3: Lớp Học — Video Learning

### Purpose
Video gallery organized by categories with search functionality.

### Layout

**3.1 Header area**
- H1: "Lớp Học Dinh Dưỡng"
- Subtitle: description text
- Search bar (frost-glass input with search icon)
- Category filter pills (frost-glass, horizontally scrollable)

**Mock categories:** Dinh dưỡng cơ bản, Vitamin & Khoáng chất, Thực đơn cho bé, Chuyên gia tư vấn

**3.2 Video Grid**
- 3-column grid (responsive: 2 on tablet, 1 on mobile)
- Each video card (leaf-glass):
  - Thumbnail placeholder (16:9 ratio) with play button overlay
  - Video title
  - Duration badge (frost-glass pill)
  - Category tag
  - Brief description (2 lines max)

**3.3 Video Detail (modal or inline expand)**
- Video player placeholder (16:9)
- Title, description
- Related videos sidebar/row

**Mock videos (6-9 items):**
- "Tầm quan trọng của Vitamin D3 cho trẻ"
- "Cách xây dựng thực đơn dinh dưỡng cho bé 1-3 tuổi"
- "Vitamin K2 và sự phát triển xương"
- "Hỏi đáp cùng bác sĩ: Miễn dịch trẻ nhỏ"
- "5 dấu hiệu bé thiếu vitamin A"
- "Hướng dẫn bổ sung vitamin đúng cách"

---

## Module 4: Thực Hành — AI Image Generator

### Purpose
Upload child's photo, select a career, AI generates an image. For mockup: show the flow with placeholder states.

### Flow
Upload photo → Select career → Processing → View result → Download/Share

### Pages

**4.1 Upload & Options** (single page)
- Warm-glass main container
- Upload zone: dashed border area, drag-drop or click
  - Camera emoji with float animation
  - "Tải ảnh bé lên" title + "Kéo thả hoặc nhấn để chọn ảnh" subtitle
- Career selection: horizontal chip/pill group (frost-glass)
  - Mock careers: Bác sĩ, Phi hành gia, Đầu bếp, Ca sĩ, Họa sĩ, Giáo viên, Kỹ sư, Phi công
  - Selected state: accent background + accent border ring
- "Tạo ảnh bằng AI" full-width accent CTA button

**4.2 Processing State**
- Centered loading animation
- "AI đang tạo ảnh..." text
- Estimated time indicator
- Leaf-glass container

**4.3 Result**
- Generated image placeholder (large, centered, leaf-glass frame)
- Career label badge
- Two CTAs: "Tải về" (accent button with download icon) + "Thử nghề khác" (ghost button)
- "Chia sẻ" option
- Small text: "Gửi ảnh dự thi giải thưởng →" link to Awards module

---

## Module 5: Giải Thưởng — Award & Contest Gallery

### Purpose
Gallery of AI-generated images, ranked by awards. Award-winning images have attached inspiration videos.

### Layout

**5.1 Header**
- H1: "Giải Thưởng & Cuộc Thi"
- Subtitle
- Filter tabs: Tất cả, Giải Nhất, Giải Nhì, Giải Ba, Yêu thích

**5.2 Gallery Grid**
- Masonry or uniform grid (3-4 columns)
- Each item (leaf-glass card):
  - AI-generated image placeholder
  - Award badge (if winner): gold/silver/bronze frost-glass pill
  - Child name + career chosen
  - Ticket/entry number
  - If award winner: play button overlay → opens video popup

**5.3 Video Popup (modal)**
- When clicking award-winning image → modal with inspiration video
- Video player placeholder
- Story text area
- Close button

**Mock items (8-12):**
- Mix of award winners and regular entries
- Various careers
- 3 winners with video badges

**5.4 Submit CTA**
- Bottom section encouraging users to submit their own
- "Tham gia cuộc thi" button → links to AI Image Generator module

---

## Module 6: Admin & CMS Dashboard

### Purpose
Mockup of admin panel — show layout and what data fields client needs to manage. Not functional, just visual prototype.

### Layout

**6.1 Sidebar Navigation**
- Logo + "WelKids Admin"
- Nav items with icons:
  - Dashboard (overview)
  - Người dùng (users)
  - Nội dung (content)
  - Video
  - Ảnh AI (AI images)
  - Giải thưởng (awards)
  - Cài đặt (settings)
- Collapsed state for mobile

**6.2 Dashboard Overview**
- Top stats cards (4 columns):
  - Tổng người dùng, Lượt kiểm tra sức khỏe, Video đã xem, Ảnh AI đã tạo
  - Each card: icon + number + label + trend indicator
- Charts area: placeholder boxes labeled "Biểu đồ lượt truy cập", "Thống kê theo module"
- Recent activity list

**6.3 Content Management Pages** (mockup list views)
- Table layout with columns appropriate to each module
- Search bar + filter dropdowns
- Pagination
- Action buttons (Xem, Sửa, Xóa) — non-functional

**Pages to mock:**
- Users list: Name, Email, Phone, Date registered, Status
- Videos list: Title, Category, Duration, Views, Status
- AI Images list: User, Career, Date, Status (pending/approved)
- Awards list: Entry, User, Career, Award level, Video attached

**6.4 Settings page**
- Form layout with sections: Thông tin chung, Campaign config, Email templates
- All fields are placeholder inputs showing what data is needed

### Admin Style
- Same design system but adapted:
  - White background instead of cream
  - Sidebar: `#2D4A35` (dark green, like footer)
  - Cards: light shadow, no glass effect (cleaner for data-heavy UI)
  - Tables: clean, alternating row colors with `#F0F5F1`

---

## Site Structure (Astro)

```
src/
├── layouts/
│   ├── BaseLayout.astro      # HTML shell, fonts, global CSS
│   ├── MainLayout.astro      # Navbar + Footer wrapper
│   └── AdminLayout.astro     # Sidebar + Admin wrapper
├── components/
│   ├── Navbar.astro
│   ├── Footer.astro
│   ├── Button.astro
│   ├── Card.astro
│   ├── GlassPanel.astro
│   ├── ChoiceCard.astro
│   ├── ProgressBar.astro
│   ├── VideoCard.astro
│   ├── GalleryItem.astro
│   ├── AdminSidebar.astro
│   ├── StatsCard.astro
│   └── DataTable.astro
├── pages/
│   ├── index.astro            # Homepage
│   ├── health-check/
│   │   ├── index.astro        # Intro
│   │   ├── quiz.astro         # Questionnaire
│   │   └── results.astro      # Results
│   ├── videos/
│   │   └── index.astro        # Video gallery
│   ├── ai-generator/
│   │   └── index.astro        # AI image generator
│   ├── awards/
│   │   └── index.astro        # Award gallery
│   └── admin/
│       ├── index.astro        # Dashboard
│       ├── users.astro
│       ├── videos.astro
│       ├── images.astro
│       ├── awards.astro
│       └── settings.astro
├── styles/
│   ├── global.css             # Design system tokens, glass effects, animations
│   └── admin.css              # Admin-specific overrides
└── data/
    ├── questions.json         # Health check mock questions
    ├── videos.json            # Mock video data
    ├── careers.json           # Career options
    └── gallery.json           # Mock gallery items
```

## Responsive Breakpoints

| Breakpoint | Width | Columns |
|-----------|-------|---------|
| Mobile | < 640px | 1 |
| Tablet | 640-1024px | 2 |
| Desktop | > 1024px | 3-4 |

Navbar collapses to hamburger on mobile. Admin sidebar collapses to icons.

## Out of Scope

- Backend logic / API
- Real AI image generation
- Real video streaming
- Authentication
- Database
- Payment

All interactions are client-side only with mock/placeholder data.
