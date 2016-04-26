## Frontend Requirements

### Home Page
- Must show list of albums<sup>[1](#1-album)</sup>
- Must have popular, new, and curated sections

### Profile Page
- Must show user information
- Must show list of owned<sup>[2](#2-owned)</sup> albums
- The logged-in current user must be able to edit their profile

### Album Page
- Must show list of chapters<sup>[3](#3-chapter)</sup>
- Logically sorted
- Must have [comments section](#comments-section)
- Must have [cover picture picker](#image-picker)
- Must be able to add or edit chapters

### Comic Chapter Viewer Page
- Must have [image viewer](#image-viewer)
- Must have comments section

### Comic Chapter Editor Page
- Must be able to add new images or change existing images via [image picker](#image-picker)
- Images must be sortable
- Must be able to pick existing or create new album on the fly
- Must not be able to upload novel chapter

### Novel Chapter Viewer Page
- Must be able to view markdown formatted text<sup>[4](#4-formatted-text)</sup>
- Must have comments section

### Novel Chapter Editor Page
- Must have WYSIWYG text editor
- Must result in markdown formatted text
- Must not be able to upload comic chapter

### Site Wide Administrative Page
- Must be able to configure overall looks of cowmilk client
- Must have [configurable Facebook login options](#configurable-login-options)
- Must have [configurable email sending options](#configurable-email-options)
- Conform to [backend's metas spec](Backend%20Requirements.md#metas)

### Per-User Administrative Page
- Must be able to [manage user information](#profile-page)
- Must be able to add new or modify existing albums 

### Site-wide
- Must have search function

### Comments Section
- Available by default
- Can be switched off
- Must be able to choose between disqus or [built-in comments viewer](#built-in-comments-viewer)

### Built-in Comments Viewer
- Linear comments flow<sup>[5](#5-linear-comments-flow)</sup>
- Must be able to reply comments
- Only writable by logged-in members

### Image Viewer
- Must view one image at a time
- Must have navigation to view other images in a chapter

### Image Picker
- An image uploader and viewer
- Must have configurable style dependent on usage

### Configurable Login Options
- A list of supported login providers
- Defaults to email and password login
- Toggleable on / off
- At least one login provider is on

### Configurable Email Options
- Defaults to empty
- [List of supported email providers](Backend%20Requirements.md#services)

## Glossary

##### 1. Album
Collection of chapters
##### 2. Owned
Album / chapter uploaded by the logged-in current user.
##### 3. Chapter
A collection of comic images or a single novel chapter.
##### 4. Formatted Text
Text that contains special formatting for pretty visual.
##### 5. Linear Comments Flow
A commenting system like 4chan. No pyramid structure, only mentions.