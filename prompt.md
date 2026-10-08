MASTER IMPLEMENTATION PROMPT

Professional Web File Manager — Stitch UI → Full-Stack Implementation

You are the primary engineering AI responsible for turning an existing Stitch-designed web file manager UI/UX prototype into a fully functional, production-ready web application.

The Stitch design is already complete.

Your job is to IMPLEMENT THE EXISTING DESIGN, not redesign it.

---

1. ABSOLUTE RULE — UI IS LOCKED

The existing Stitch design is the source of truth for the frontend.

DO NOT:

- Redesign the UI
- Change the visual style
- Replace layouts
- Change spacing unnecessarily
- Change typography
- Change colors
- Change border radii
- Change iconography
- Replace the sidebar
- Replace the toolbar
- Change file cards
- Change list view
- Change dialogs
- Change context menus
- Change responsive layouts
- Add random UI components
- Convert the application into a generic SaaS dashboard

DO:

- Preserve the Stitch UI
- Preserve component hierarchy
- Preserve interactions
- Preserve responsive behavior
- Connect real functionality to existing UI
- Add only UI elements that are strictly necessary for functionality
- Keep any necessary additions visually consistent with the existing design system

Think:

«Stitch = visual source of truth

You = engineering implementation»

If there is a conflict between your preferred UI and the Stitch UI, Stitch wins.

---

2. FIRST STEP — ANALYZE THE EXISTING PROJECT

Before modifying code:

1. Inspect the entire existing project.
2. Identify the framework.
3. Identify the frontend architecture.
4. Identify existing components.
5. Identify routing.
6. Identify styling system.
7. Identify environment configuration.
8. Identify existing database configuration.
9. Identify existing API/service layers.
10. Identify the Stitch-generated screens and components.
11. Identify reusable components.
12. Identify mock/sample data.
13. Identify placeholder event handlers.

Do NOT immediately rewrite the project.

First understand what already exists.

Create a short internal implementation plan based on the existing project structure.

Then implement incrementally.

---

3. PRIMARY OBJECTIVE

Turn the existing file manager prototype into a real application capable of:

- User authentication
- File upload
- File download
- Folder creation
- Folder navigation
- File navigation
- Rename
- Copy
- Move
- Delete
- Trash
- Restore
- Permanent deletion
- Search
- Filtering
- Sorting
- Grid view
- List view
- File preview
- File metadata
- Favorites
- Recent files
- Shared files
- Storage usage
- User settings
- Keyboard shortcuts
- Multi-selection
- Drag-and-drop
- Upload progress
- Error handling
- Permissions
- Secure file access

---

4. DO NOT ASSUME A TECHNOLOGY STACK

Use the technologies already present in the project whenever practical.

If the project already uses:

- React → keep React
- Next.js → keep Next.js
- TypeScript → keep TypeScript
- Tailwind → keep Tailwind
- Supabase → use the existing Supabase setup
- PostgreSQL → use the existing PostgreSQL setup
- Firebase → use the existing Firebase setup

Do not replace the entire stack simply because you prefer another technology.

Only introduce a new technology when there is a clear engineering reason.

---

5. ARCHITECTURE

Use a clean separation between:

UI
 ↓
Frontend state
 ↓
Service/API layer
 ↓
Backend
 ↓
Database
 ↓
File storage

The UI components should not contain unnecessary backend logic.

Create clear service boundaries.

Example conceptual structure:

components/
pages/
routes/
services/
lib/
hooks/
types/
utils/
server/
database/
storage/

Adapt this structure to the existing project rather than blindly creating duplicate architecture.

---

6. DATABASE MODEL

Design a robust database model.

At minimum support:

Users

id
email
name
avatar
created_at
updated_at

Files

id
owner_id
parent_folder_id
name
extension
mime_type
size
storage_key
created_at
updated_at
last_accessed_at
deleted_at
is_favorite

Folders

id
owner_id
parent_folder_id
name
created_at
updated_at
deleted_at
is_favorite

Shares

id
file_id
folder_id
owner_id
shared_with_user_id
permission
created_at
expires_at

Permissions should support at least:

viewer
editor
owner

Recent Files

Track:

user_id
file_id
accessed_at

Tags

Support future extensibility for:

Important
Work
Personal
Projects

Storage Usage

Track or calculate:

user_id
used_bytes
quota_bytes

Use database constraints and indexes appropriately.

---

7. FILE STORAGE

Use a proper object/file storage system rather than storing large files directly in the database.

Storage architecture should support:

- Unique storage keys
- Secure access
- Upload
- Download
- Delete
- Restore
- Large files
- File metadata
- Future cloud storage expansion

Never expose private storage credentials to the client.

Never place secret storage keys in frontend code.

---

8. AUTHENTICATION

Implement secure authentication.

Support:

- Sign up
- Login
- Logout
- Session persistence
- Password reset if supported by the chosen authentication system

Protect private file-manager routes.

A user must only be able to access resources they are authorized to access.

Do not expose another user's:

- Files
- Folders
- Storage
- Shares
- Metadata

---

9. FILE SYSTEM MODEL

Implement hierarchical folders.

Example:

Home
├── Documents
│   ├── Work
│   │   ├── Project A
│   │   └── Project B
│   └── Personal
├── Downloads
├── Pictures
└── Videos

Each folder must have:

- Unique ID
- Parent folder
- Owner
- Name

Avoid relying solely on filesystem paths as identifiers.

Use stable IDs.

---

10. NAVIGATION

Connect the existing Stitch navigation UI to real data.

Support:

- Home
- Desktop
- Documents
- Downloads
- Pictures
- Music
- Videos
- Recent
- Favorites
- Shared
- Trash
- Storage
- Custom folders

Breadcrumbs must dynamically represent the current folder.

Example:

Home / Documents / Work / Project A

Clicking a breadcrumb should navigate to the correct folder.

Back/forward navigation must work correctly.

---

11. FILE OPERATIONS

Implement:

Create folder

- Validate name
- Prevent invalid names
- Prevent duplicate names where appropriate
- Create database record
- Refresh current view

Rename

- Inline rename using existing Stitch UI
- Validate name
- Preserve extension appropriately
- Update database

Copy

Support:

- File copy
- Folder copy
- Recursive folder copy

Avoid name collisions.

Example:

Project
Project (1)
Project (2)

Move

Support:

- File movement
- Folder movement
- Recursive folder movement
- Prevent moving a folder inside itself
- Update parent relationship

Delete

Default behavior:

«Move to Trash»

Do not permanently delete immediately unless explicitly requested.

---

12. TRASH

Implement a real Trash system.

When deleted:

deleted_at = current_timestamp

The item should disappear from normal views.

Trash should display:

- Name
- Original location
- Deleted date
- Size

Actions:

- Restore
- Permanently delete
- Empty Trash

Restoring should return the item to its previous valid location.

Handle the case where the original parent folder no longer exists.

---

13. SEARCH

Implement real search.

Search across:

- File names
- Folder names
- Extensions
- MIME types
- Locations

Support filters:

- Type
- Date
- Size
- Location
- Favorites

Search should be performant.

Do not load every file into the browser just to search it.

Use server-side/database queries when appropriate.

---

14. SORTING

Support:

- Name
- Date modified
- Date created
- Type
- Size

Support ascending/descending order.

Persist the user's preferred sort setting when appropriate.

---

15. GRID / LIST VIEW

The Stitch design already contains both.

Connect both to the same data source.

Changing view should not reload the entire application unnecessarily.

Persist the user's preferred view mode.

Example:

Grid
List

---

16. MULTI-SELECTION

Implement:

- Single selection
- Ctrl/Cmd selection
- Shift selection
- Select all
- Clear selection

Connect the existing contextual action bar to real actions.

Actions:

- Download
- Share
- Copy
- Move
- Delete
- More

Make sure actions correctly handle mixed selections of files and folders.

---

17. DRAG AND DROP

Implement:

Upload

Dragging files from the user's computer into the file manager should trigger upload.

Move

Dragging existing files/folders between folders should move them.

Clearly distinguish:

Upload
Move

Prevent invalid operations.

---

18. UPLOAD SYSTEM

Implement real uploads.

Support:

- Multiple files
- Large files
- Progress
- Cancellation
- Retry
- Failure handling
- Duplicate handling
- Drag-and-drop

Connect the existing Stitch upload panel to real progress.

Example:

Uploading 3 files

Resume.pdf
87%

Project.zip
52%

Photo.jpg
Completed

Do not fake progress.

Progress should represent actual upload state.

---

19. DOWNLOAD

Implement secure downloads.

For private files:

- Verify user permission
- Generate a secure download mechanism
- Do not expose unrestricted storage URLs

Support downloading individual files.

For folders:

If folder download is supported, package it appropriately, such as creating a ZIP.

Do not block the UI unnecessarily.

---

20. FILE PREVIEW

Connect the existing preview panel.

Support appropriate previews for:

Images

JPEG, PNG, WebP, GIF, etc.

PDFs

Display a PDF preview.

Text

Display safe text preview.

Video

Provide video preview where supported.

Audio

Provide audio playback where appropriate.

Unsupported files

Display:

Preview unavailable

Download this file to open it.

Never execute arbitrary uploaded files in the browser.

---

21. FILE INFORMATION

Connect the existing Properties/Information UI.

Display:

- Name
- Type
- Size
- Created
- Modified
- Location
- Dimensions
- Duration
- Sharing
- Permissions

Only display metadata that actually exists.

---

22. FAVORITES

Implement:

- Add to Favorites
- Remove from Favorites
- Favorites page
- Persistent favorite state

Support both files and folders.

---

23. RECENT FILES

Track file access.

When a user opens/previews a file:

last_accessed_at

Update Recent Files.

Prevent unlimited duplicate recent records.

Use a sensible recent-file limit.

---

24. SHARING

Implement the existing Shared Files UI.

Users should be able to share files/folders with another user.

Permissions:

Viewer
Editor

Viewer:

- Read
- Preview
- Download

Editor:

- Read
- Preview
- Download
- Rename
- Move where permitted
- Modify where appropriate

Never allow shared users to access unrelated private resources.

---

25. STORAGE MANAGEMENT

Connect the existing storage UI.

Display:

342 GB used
512 GB total
170 GB available

Calculate storage based on actual stored files.

Break down by:

- Documents
- Images
- Videos
- Other

Avoid expensive repeated full-database calculations where possible.

Cache or aggregate storage metrics when appropriate.

---

26. SETTINGS

Connect the existing settings UI.

Support:

General

- Default view
- Confirm deletion
- Open folders in new tab
- Show extensions

Appearance

- Light
- Dark
- System
- Compact
- Comfortable

Files

- Sort preference
- Hidden files
- Download preferences

Persist settings per user where appropriate.

---

27. KEYBOARD SHORTCUTS

Implement:

Ctrl/Cmd + A → Select all
Ctrl/Cmd + C → Copy
Ctrl/Cmd + X → Cut
Ctrl/Cmd + V → Paste
Ctrl/Cmd + F → Search
F2 → Rename
Delete → Move to Trash
Enter → Open
Backspace → Go back

Do not interfere with normal browser shortcuts unnecessarily.

Only intercept shortcuts when the file manager has appropriate focus/context.

---

28. CONTEXT MENUS

Connect the existing Stitch context menus to real operations.

File:

Open
Preview
Download
Share
Copy
Move
Rename
Add to Favorites
Get Info
Delete

Folder:

Open
New File
New Folder
Copy
Move
Rename
Share
Add to Favorites
Get Info
Delete

Disable actions that are unavailable.

---

29. ERROR HANDLING

Never silently fail.

Display useful errors.

Examples:

Unable to upload file.

Try again.

You don't have permission to access this file.

This folder no longer exists.

The file is too large.

Do not expose:

- Stack traces
- Database errors
- Secrets
- Internal implementation details

to users.

Log technical details securely on the server.

---

30. SECURITY

Treat uploaded files as untrusted input.

Implement appropriate protection against:

- Path traversal
- Unauthorized file access
- IDOR
- Malicious filenames
- MIME spoofing
- XSS through file metadata
- SQL injection
- CSRF where relevant
- Unauthorized sharing
- Privilege escalation

Never trust:

- Client-provided user IDs
- Client-provided ownership
- Client-provided file paths
- Client-provided permissions
- Client-provided file sizes

Validate on the server.

---

31. FILE NAME SECURITY

Sanitize and validate filenames.

Handle:

- Empty names
- Reserved names
- Extremely long names
- Special characters
- Unicode
- Duplicate names
- Malicious strings

Display filenames safely.

Never inject filenames directly into HTML.

---

32. PERFORMANCE

The application should remain fast with large numbers of files.

Plan for:

- Hundreds
- Thousands
- Potentially tens of thousands of files

Use:

- Pagination or cursor-based loading
- Lazy loading
- Virtualized lists where appropriate
- Server-side search
- Efficient queries
- Indexed database columns
- Thumbnail optimization
- Caching where appropriate

Do not fetch every file record on application startup.

---

33. RESPONSIVE BEHAVIOR

Preserve the Stitch mobile and tablet designs.

Do not replace the responsive design with a generic mobile dashboard.

Desktop:

- Full sidebar
- Toolbar
- File grid/list
- Optional preview panel

Tablet:

- Collapsible sidebar
- Adaptive layout

Mobile:

- Drawer navigation
- Touch-friendly controls
- Bottom sheets where already designed
- Mobile file browsing

---

34. DARK MODE

Preserve Stitch's dark theme.

Do not redesign it.

Ensure dynamically generated components also use the existing design tokens.

---

35. TOASTS

Connect existing toast UI to actual operations.

Examples:

File uploaded successfully
Folder created
File renamed
3 files moved to Trash
File copied
Download started
Upload failed

Do not show success messages if an operation actually failed.

---

36. LOADING STATES

Use the existing Stitch skeleton/loading states.

Show loading indicators during:

- Folder navigation
- Search
- Upload
- Download preparation
- Preview loading
- File operations

Avoid unnecessary full-screen loading.

Prefer local loading states.

---

37. OPTIMISTIC UI

Use optimistic updates only when safe.

Good examples:

- Favorite/unfavorite
- Rename
- Move
- Delete

If the server operation fails:

1. Roll back the UI.
2. Show an error.
3. Restore the previous state.

Never allow the frontend to permanently diverge from the backend.

---

38. API DESIGN

Create clean APIs/services for operations such as:

GET    /files
GET    /folders/:id
POST   /folders
PATCH  /files/:id
PATCH  /folders/:id
DELETE /files/:id
POST   /files/:id/restore
POST   /files/:id/copy
POST   /files/:id/move
POST   /upload
GET    /download/:id
GET    /search
POST   /share
DELETE /share/:id
GET    /recent
GET    /favorites
GET    /trash
GET    /storage

Adapt these routes to the actual framework rather than blindly implementing these exact URLs.

Keep API responses predictable and typed.

---

39. TYPE SAFETY

Use strong types throughout.

Create shared types for:

User
File
Folder
Share
Permission
Storage
RecentFile
Tag
Upload

Avoid unnecessary "any".

Validate external/API data at boundaries.

---

40. DATABASE INTEGRITY

Enforce important rules at the database level wherever possible.

Examples:

- Valid ownership
- Valid parent relationships
- Unique IDs
- Foreign keys
- Permission relationships
- Valid share targets

Do not rely exclusively on frontend validation.

---

41. TESTING

Create tests for critical operations.

At minimum test:

Authentication

- Login
- Logout
- Unauthorized access

Files

- Upload
- Download
- Rename
- Delete
- Restore

Folders

- Create
- Rename
- Move
- Delete
- Restore

Permissions

- Owner access
- Viewer access
- Editor access
- Unauthorized access

Search

- Filename search
- Folder search
- Filters

Security

- Cross-user access attempts
- Invalid IDs
- Path traversal attempts

---

42. EDGE CASES

Handle:

- Empty folders
- Empty search
- Duplicate filenames
- Deleted parent folder
- Missing file
- Failed upload
- Interrupted upload
- Large files
- Unsupported file type
- Network failure
- Expired session
- Unauthorized access
- Moving folder into itself
- Sharing with invalid user
- Storage quota exceeded
- Concurrent modifications

Do not ignore edge cases.

---

43. REALISTIC SAMPLE DATA

Keep the existing Stitch sample data for development/demo purposes where useful.

Examples:

Documents
Projects
Downloads
Pictures
Videos
Music
Work
Personal
Archive

Resume.pdf
Project Proposal.docx
Budget.xlsx
Presentation.pptx
Vacation.jpg
Demo.mp4
Notes.txt
Website.zip

Production mode should use real user data.

---

44. UI INTEGRATION RULE

When functionality requires a missing UI element:

1. First check whether an existing Stitch component can represent the state.
2. Reuse it.
3. Only create a new UI component if absolutely necessary.
4. Match the existing Stitch design system exactly.

Example:

If upload errors need an error state, use the existing upload/error design rather than inventing a new modal.

---

45. DO NOT REPLACE THE DESIGN WITH DEFAULT COMPONENTS

Do not suddenly introduce:

- Generic Material UI dialogs
- Generic Bootstrap tables
- Generic dashboard cards
- Default browser alerts
- Default browser prompts
- Unstyled file inputs
- Generic admin layouts

Everything should visually belong to the Stitch-designed product.

---

46. ACCESSIBILITY

Maintain:

- Keyboard navigation
- Focus states
- Screen-reader labels
- Semantic HTML
- Accessible dialogs
- Accessible menus
- Accessible buttons
- Appropriate contrast

Do not sacrifice accessibility for appearance.

---

47. DEPLOYMENT READINESS

Prepare the application for production.

Use environment variables for:

- Database credentials
- Storage credentials
- Authentication secrets
- API keys
- Application secrets

Never commit secrets.

Create appropriate:

- Production build
- Environment configuration
- Database migrations
- Error logging
- Secure headers where applicable
- Rate limiting where appropriate

---

48. IMPLEMENTATION STRATEGY

Work in phases.

PHASE 1 — Understand

Analyze the Stitch-generated project.

Do not modify the UI unnecessarily.

PHASE 2 — Foundation

Set up:

- Database
- Authentication
- Storage
- Core models
- API/service layer

PHASE 3 — File System

Implement:

- Files
- Folders
- Navigation
- CRUD
- Trash

PHASE 4 — UI Integration

Connect the existing Stitch components to real data.

PHASE 5 — Advanced Features

Implement:

- Search
- Sharing
- Favorites
- Recent
- Preview
- Storage

PHASE 6 — Polish

Implement:

- Loading
- Errors
- Toasts
- Keyboard shortcuts
- Drag/drop
- Responsive behavior

PHASE 7 — Security

Perform a complete security review.

PHASE 8 — Testing

Run unit/integration/end-to-end tests.

PHASE 9 — Production

Prepare deployment configuration and documentation.

---

49. DEVELOPMENT RULE

Do not rewrite large portions of the project unnecessarily.

Prefer:

Understand → Extend → Integrate → Test

over:

Delete → Rewrite everything

Preserve working code whenever possible.

---

50. FINAL ACCEPTANCE CRITERIA

The application is complete only when:

UI

- Stitch design remains visually intact.
- All screens use one consistent design system.
- Desktop/tablet/mobile layouts work.

File management

- Files can be uploaded.
- Files can be downloaded.
- Folders can be created.
- Files/folders can be renamed.
- Files/folders can be copied.
- Files/folders can be moved.
- Items can be deleted.
- Trash works.
- Restore works.
- Permanent deletion works.

Organization
Search works.
Sorting works.
Filtering works.
Grid/list views work.
Favorites work.
Recent files work.
Shared files work.
Preview
Supported files preview correctly.
Unsupported files show a graceful fallback.
Security
Users cannot access other users' private files.
Permissions are enforced server-side.
Storage credentials remain private.
Uploaded files are treated as untrusted.
Performance
Large directories remain responsive.
Search remains efficient.
Upload progress is real.
UI does not unnecessarily reload.
Reliability
Errors are handled.
Loading states work.
Failed operations can recover.
Database remains consistent.
UX
Keyboard shortcuts work.
Drag-and-drop works.
Context menus work.
Selection works.
Responsive layouts work.
51. MOST IMPORTANT INSTRUCTION
Remember this throughout the entire implementation:
DO NOT REDESIGN THE STITCH UI.
The Stitch prototype is the approved product design.
Your responsibility is to make that design real, functional, secure, performant and production-ready.
If you believe a UI change is necessary for technical reasons, do not arbitrarily redesign it. Make the smallest possible change while preserving the existing design language.
The final result should look almost identical to the Stitch prototype while behaving like a complete professional file-management application.
FINAL GOAL
Deliver:
Stitch-quality UI + real file-management functionality + secure backend + persistent database + real file storage + production-ready architecture
The user should be able to open the application and feel that they are using a polished desktop file manager — except it runs entirely in the web browser.
