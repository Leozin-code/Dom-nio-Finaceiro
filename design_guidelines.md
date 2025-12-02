# Design Guidelines: School Grade Management System

## Design Approach

**Selected Framework:** Material Design 3 (Material You)  
**Rationale:** Ideal for data-heavy educational applications requiring clear information hierarchy, accessible form patterns, and professional appearance. Material's established components handle complex tables, multi-step forms, and data visualization efficiently.

**Key Principles:**
- Clarity over decoration
- Consistent, predictable patterns
- Accessibility-first approach
- Efficient data entry and retrieval

---

## Typography

**Font Family:** Roboto (primary), Inter (alternative)
- **Headings:** Roboto Medium/Bold - sizes: text-3xl, text-2xl, text-xl
- **Body Text:** Roboto Regular - text-base (16px)
- **Data Tables:** Roboto Regular - text-sm (14px)
- **Labels/Captions:** Roboto Medium - text-xs, text-sm
- **Numbers/Grades:** Roboto Mono for grade displays

---

## Layout System

**Spacing Scale:** Tailwind units of **2, 4, 6, 8, 12, 16** (e.g., p-4, gap-6, m-8)

**Container Strategy:**
- Main content area: max-w-7xl with px-6
- Forms: max-w-2xl centered
- Data tables: full-width within container
- Cards: p-6 for content padding

**Grid Patterns:**
- Dashboard stats: grid-cols-1 md:grid-cols-2 lg:grid-cols-4
- Form layouts: grid-cols-1 md:grid-cols-2 with gap-6
- Student/teacher lists: Single column tables with full details

---

## Component Library

### Navigation
**Admin Sidebar:**
- Fixed left sidebar (w-64) with collapsible mobile drawer
- Vertical navigation with icon + label pattern
- Active state indicator (background highlight + border-l-4)
- Sections: Dashboard, Alunos, Professores, Turmas, Disciplinas, Notas, Avisos, Configurações
- Bottom: User profile + logout

**Top Bar:**
- User name and role badge (right-aligned)
- Logout button
- Mobile: Hamburger menu trigger

**Student Navigation:**
- Simplified horizontal tab navigation (Notas, Avisos, Perfil)
- Cleaner, less complex than admin interface

### Data Display

**Tables:**
- Striped rows for readability (alternate subtle background)
- Sticky headers for long lists
- Row actions (edit/delete) aligned right
- Pagination controls at bottom
- Search/filter bar above table
- Empty states with helpful guidance

**Cards:**
- Rounded corners (rounded-lg)
- Subtle shadow (shadow-sm)
- Padding p-6
- Use for dashboard widgets, summary panels, and student grade cards

**Grade Display:**
- Large numerical display for individual grades
- Visual indicators:
  - Green (≥7.0): Approved
  - Yellow (6.0-6.9): At risk
  - Red (<6.0): Below minimum
- Progress bars for subject averages
- Badge components for status (Aprovado/Em Risco/Reprovado)

### Forms

**Input Fields:**
- Full-width within form container
- Floating labels (Material style) or top-aligned labels
- Helper text below inputs for guidance
- Error states with red border + error message
- Required field indicators (asterisk)

**Form Layouts:**
- Two-column grid on desktop (md:grid-cols-2)
- Logical grouping with fieldset/legend
- Clear primary action button (bottom right)
- Secondary actions (Cancel) less prominent

**Select/Dropdowns:**
- Custom styled to match design system
- Multi-select with chip display for selected items
- Searchable for long lists (turmas, disciplinas)

**Date Pickers:**
- Calendar interface for evaluation dates
- Clear formatting (DD/MM/YYYY)

### Buttons

**Primary Actions:** 
- Solid background, medium height (h-10, h-11)
- Padding px-6
- Clear labels: "Cadastrar Aluno", "Lançar Notas", "Enviar Aviso"

**Secondary Actions:**
- Outline style with transparent background
- Used for cancel, back navigation

**Icon Buttons:**
- Square (w-10 h-10) for table row actions
- Tooltips on hover for clarity

### Status Indicators

**Badges:**
- Rounded-full with px-3 py-1
- Status-specific backgrounds (green/yellow/red with opacity)
- Small text (text-xs)

**Alerts/Notifications:**
- Toast messages for success/error feedback
- Positioned top-right
- Auto-dismiss after 4 seconds
- Icon + message + close button

---

## Dashboard Layouts

### Admin Dashboard
- **Stats Row:** 4-column grid showing total students, classes, subjects, recent notices
- **Charts Section:** 2-column layout with average grades by class (bar chart) and students below average (pie chart)
- **Recent Activity:** Table of latest grade entries or notices sent
- **Quick Actions:** Card with prominent buttons for common tasks

### Student Dashboard
- **Welcome Header:** Student name + class prominently displayed
- **Grade Summary:** Large cards showing overall average and subject breakdown
- **Recent Notices:** List of 3-5 most recent announcements
- **Subject Cards:** Grid showing each subject with current grade and trend indicator

---

## Responsive Behavior

**Breakpoints:**
- Mobile: Base styles (single column, stacked navigation)
- Tablet: md: (2-column forms, simplified table columns)
- Desktop: lg: (full sidebar, multi-column dashboards, complete tables)

**Mobile Adaptations:**
- Sidebar becomes drawer (slide from left)
- Tables show priority columns only, expand for details
- Forms stack to single column
- Dashboard stats stack vertically

---

## Data Entry Workflows

**Grade Entry Interface:**
1. Filter selection (Turma → Disciplina → Avaliação) using dropdowns
2. Student table appears with name + current grade
3. Inline editing: Click grade cell to edit
4. Bulk save button at bottom
5. Visual validation (prevent >10 grades immediately)

**Notice Sending:**
- Simple form: Title, Content (textarea), Recipient selector (radio: All/Class/Individual)
- Conditional dropdowns based on recipient type
- Preview section showing who will receive
- Send confirmation dialog

---

## Visual Hierarchy

**Emphasis Levels:**
1. **Primary:** Dashboard stats, grade displays, CTA buttons
2. **Secondary:** Form section headers, table headers, subject names
3. **Tertiary:** Helper text, metadata (dates, timestamps), secondary info

**Spacing Rhythm:**
- Section spacing: mb-12 or mb-16 between major sections
- Component spacing: gap-6 between related items
- Form field spacing: gap-4 within forms
- Table row height: h-16 for comfortable click targets

---

## Accessibility Standards

- Minimum touch target: 44x44px for mobile
- Color contrast ratio: WCAG AA minimum (4.5:1 for text)
- Keyboard navigation: All interactive elements accessible via Tab
- Form validation: Clear error messages, not just color indicators
- Screen reader: Semantic HTML, proper ARIA labels for tables and forms

---

## Images

**No hero images required** - This is a utility application focused on data management.

**Icons:** Use Material Icons via CDN for consistent system icons throughout (navigation, actions, status indicators)

**Profile Photos:** Circular avatars (48x48px) for student/teacher profiles in lists and headers