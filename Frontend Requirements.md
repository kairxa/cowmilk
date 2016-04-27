## Frontend Requirements

### Home Page
- Shows list of albums<sup>[1](#1-album)</sup>
- Has popular, new, and curated albums sections
- Has album search function

### Profile Page
- Shows user information
- Shows list of owned<sup>[2](#2-owned)</sup> albums
- Profile owner is able to edit their profile

### Album Page
- Shows list of chapters<sup>[3](#3-chapter)</sup>
- Has capability to sort chapters
- Has [comments section](#comments-section)
- Has [cover picture picker](#image-picker)
- Has chapter search function
- Album owner is able to add or edit chapters

### Comic Chapter Viewer Page
- Has [image viewer](#image-viewer)
- Has comments section

### Comic Chapter Editor Page
- The artist is able to add new or change existing images via [image picker](#image-picker)
- Images are sortable
- The artist is able to pick existing or create new album on the fly

### Novel Chapter Viewer Page
- Views markdown formatted text<sup>[4](#4-formatted-text)</sup>
- Has comments section

### Novel Chapter Editor Page
- Has WYSIWYG text editor
- WYSIWYG editor makes markdown formatted text

### Site Wide Administrative Page
- The site administrator is able to configure overall looks of cowmilk client via preset options
- Has [configurable Facebook login options](#configurable-login-options)
- Has [configurable email sending options](#configurable-email-options)
- List of supported fields conforms to [backend's metas spec](Backend%20Requirements.md#metas)

### Comments Section
- Available by default
- Can be switched off
- The owner is able to choose between comments provider<sup>[5]</sup>

### Built-in Comments Viewer
- Linear comments flow<sup>[6](#6-linear-comments-flow)</sup>
- Every logged-in user is able to add or reply comments

### Image Viewer
- Views one image at a time
- Has navigation to view other images in a chapter

### Image Picker
- Image picker is an image uploader and viewer
- Image picker is a standalone component, so its style must be configurable dependent on usage

### Configurable Login Options
- Has a [list of supported login providers](Backend%20Requirements.md#services)
- Defaults to email and password login
- Toggleable on / off
- At least one login provider is on

### Configurable Email Options
- Has a [list of supported email providers](Backend%20Requirements.md#services)
- Defaults to empty
- Toggleable on / off

## Glossary

##### 1. Album
Collection of chapters
##### 2. Owned
Album / chapter uploaded by the logged-in current user.
##### 3. Chapter
A collection of comic images or a single novel chapter.
##### 4. Formatted Text
Text that contains special formatting for pretty visual.
##### 5. Comments Provider
Service like disqus or [inhouse built-in](#built-in-comments-viewer)
##### 6. Linear Comments Flow
A commenting system like 4chan. No pyramid structure, only mentions.