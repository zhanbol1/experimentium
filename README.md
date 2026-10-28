# Laboratory Teaching Platform

## Project overview

This project is a WordPress-based educational platform inspired by the general workflow of [Experimentium](https://www.experimentium.org/). It will help instructors discover laboratory experiments, assemble them into courses, generate consolidated materials, and download a complete teaching package.

The goal is to build an original product with similar capabilities—not to copy Experimentium's design, code, or restricted content.

## Product goals

The platform should allow instructors to:

1. Browse and filter laboratory experiments.
2. Preview instructions, resources, safety information, chemicals, and equipment.
3. Register and create personal courses.
4. Add, replace, remove, and reorder experiments.
5. Save, duplicate, and revise courses.
6. Generate consolidated chemical and equipment lists.
7. Download course schedules, documents, and teaching materials as a package.
8. Access rubrics, syllabus templates, safety resources, videos, and implementation guidance.

## Recommended technology

- Current stable WordPress release
- PHP 8.3 or newer
- MySQL 8.0+ or MariaDB 10.11+
- Nginx or Apache
- HTTPS
- Composer for PHP dependencies
- Git-based deployment
- A custom WordPress block theme
- A custom PHP plugin for the platform's core functionality
- Small React components for interactive features such as filtering and drag-and-drop course ordering
- WordPress REST API with cookie authentication and nonces

A traditional WordPress architecture is recommended over a fully headless application. PHP should handle business rules, permissions, data validation, document generation, and downloads. JavaScript should be limited to interactions that materially improve the user experience.

## High-level architecture

```text
WordPress
|-- Custom block theme
|   |-- Public pages
|   |-- Experiment catalogue
|   |-- Experiment detail pages
|   `-- Instructor dashboard templates
|
|-- Custom "Lab Platform" plugin
|   |-- Content types and taxonomies
|   |-- Course builder
|   |-- User roles and permissions
|   |-- REST API endpoints
|   |-- Search and filtering
|   |-- Document and ZIP generation
|   `-- Administration tools
|
|-- WordPress database
|   |-- Experiments and resources
|   |-- Taxonomies and metadata
|   `-- User courses and ordered course items
|
`-- Media storage
    |-- Documents
    |-- Images and videos
    `-- Generated PDF, spreadsheet, and ZIP files
```

Content definitions and business logic belong in the custom plugin so that changing the theme does not remove or break platform data.

## Users and permissions

### Visitor

- Browse, search, and filter experiments
- View public resources and sample materials
- Read teaching guides
- Register for an account

### Instructor

- Save favorite experiments
- Create, update, duplicate, and delete personal courses
- Reorder and replace experiments
- Generate chemical and equipment lists
- Download course packages
- Manage a personal profile

### Content editor

- Create and update experiments
- Upload and manage documents
- Manage taxonomies and course templates
- Review and publish educational content

### Administrator

- Manage users, roles, and permissions
- Configure the platform
- Manage licensing and attribution
- View reports and run maintenance operations

Authorization must use granular WordPress capabilities and object ownership checks rather than relying only on role names.

## Information architecture

### Public navigation

- Home
- Explore experiments
- Ready-made courses
- Teaching resources
- How to use the platform
- About
- FAQ
- Contact
- Log in / Register

### Instructor navigation

- Dashboard
- My courses
- Create a course
- Saved experiments
- Downloads
- Profile
- Help

### Administration

- Experiments
- Course templates
- Resources
- Chemicals
- Equipment
- Learning outcomes
- Licences and attribution
- Users
- Reports
- Settings

## Content and data model

### Experiment fields

Each experiment should support:

- Title and summary
- Cover image
- Full description
- Learning outcomes
- Subject and course level
- Duration and difficulty
- Group size and prerequisites
- Safety and waste-disposal information
- Accessibility notes
- Chemicals and quantities
- Equipment and quantities
- Instructor and student documents
- Pre-lab and data-analysis materials
- Rubrics and video resources
- Language
- Licence and attribution
- Version and revision date

### WordPress content types

Use custom post types for:

- Experiments
- Resources
- Course templates
- Teaching guides
- Contributors

Use taxonomies for:

- Subject
- Course level
- Experiment type
- Duration range
- Difficulty
- Teaching method
- Language
- Accessibility features

Chemicals and equipment may start as structured entities in the plugin. They should move to dedicated database tables if inventory, aggregation, or reporting requirements become extensive.

### User course storage

User-created courses should use normalized custom tables rather than one large serialized metadata value:

- `lab_courses`
- `lab_course_items`
- `lab_course_versions`
- `lab_generated_packages`

A course item should contain its course ID, experiment ID, position, scheduled date, instructor notes, selected document variants, and permitted customizations.

## Course-builder workflow

1. The instructor registers or logs in.
2. The instructor creates an empty course or selects a template.
3. The instructor searches the experiment catalogue.
4. Experiments are added to the course.
5. Experiments are reordered with accessible drag-and-drop controls.
6. Experiments can be replaced or removed.
7. Dates and instructor notes are added.
8. Changes are autosaved.
9. The platform validates missing documents and incomplete data.
10. A downloadable course package is generated.

An example package:

```text
General-Chemistry-I/
|-- course-overview.pdf
|-- experiment-schedule.pdf
|-- chemical-list.xlsx
|-- equipment-list.xlsx
|-- attribution-and-licences.pdf
|-- instructor-materials/
|   |-- 01-density/
|   `-- 02-titration/
`-- student-materials/
    |-- 01-density/
    `-- 02-titration/
```

The requirements phase must determine whether quantities are multiplied by student or group count, how duplicates are consolidated, which source formats are included, and whether large packages need queued background generation.

## Search and filters

The catalogue should support:

- Keyword
- Subject or topic
- Academic level
- Duration
- Difficulty
- Group size
- Equipment availability
- Chemical restrictions
- Teaching approach
- Language
- Accessibility options

Native WordPress queries are sufficient for an initially moderate catalogue. A dedicated search service should only be introduced after catalogue size or measured performance justifies it.

## Plugin structure

```text
lab-platform/
|-- src/
|   |-- Content/
|   |-- Courses/
|   |-- Catalogue/
|   |-- Downloads/
|   |-- Users/
|   |-- REST/
|   |-- Admin/
|   |-- Security/
|   `-- Infrastructure/
|-- assets/
|   |-- js/
|   `-- css/
|-- templates/
|-- tests/
`-- lab-platform.php
```

Use namespaces, Composer autoloading, WordPress coding standards, and automated tests. Use established third-party plugins for commodity features such as SEO, mail delivery, backups, two-factor authentication, caching, analytics, and multilingual content. Do not build the core course workflow from a collection of unrelated form or membership plugins.

## MVP scope

### Included

- Public experiment catalogue
- Structured experiment detail pages
- Search and filters
- Instructor registration and login
- Course templates
- Course creation and duplication
- Add, replace, remove, and reorder experiments
- Autosave
- Consolidated equipment and chemical lists
- PDF, spreadsheet, and ZIP downloads
- Administrative content management
- Licensing and attribution management
- Responsive and accessible interface

### Deferred

- Student accounts
- Assignment submissions and grading
- LMS integration
- Real-time collaboration
- Public course sharing
- Community discussions
- Advanced inventory management
- AI-generated content
- Native mobile applications
- Institutional single sign-on
- Complex multilingual document generation

The first release should be an excellent instructor resource and course-planning tool, not a complete learning management system.

## Development roadmap

The estimated MVP duration is 14–18 weeks for one experienced WordPress developer, a part-time designer, and regular support from a chemistry/content specialist.

### Phase 0: Discovery and product definition — 1–2 weeks

- Interview stakeholders and representative instructors
- Define users and primary journeys
- Audit available content and licences
- Define the experiment taxonomy
- Establish MVP boundaries and success metrics
- Confirm hosting and operational requirements
- Decide registration, content access, languages, document formats, and whether student accounts are needed

### Phase 1: UX and technical design — 2 weeks

- Produce the sitemap and user-flow diagrams
- Design wireframes
- Prototype the experiment catalogue and course builder
- Define the database schema
- Specify REST API endpoints
- Produce a permission matrix
- Write acceptance criteria

The course-builder prototype should be validated before final theme development because it is the highest-risk user interaction.

### Phase 2: WordPress foundation — 1–2 weeks

- Configure development, staging, and production environments
- Establish Git and automated deployment
- Create the custom plugin and block theme
- Register post types and taxonomies
- Create custom database tables
- Add instructor capabilities
- Configure mail, backups, HTTPS, and caching

### Phase 3: Content platform — 2–3 weeks

- Build the experiment editor
- Add structured chemical and equipment fields
- Implement document management
- Build resource and course-template editors
- Add licensing and attribution fields
- Add editorial validation
- Build catalogue and experiment pages
- Implement search and filters
- Begin normalized content entry

### Phase 4: Accounts and course builder — 3–4 weeks

- Implement registration and login
- Build the instructor dashboard
- Add course creation, renaming, duplication, and deletion
- Add experiment selection and replacement
- Implement accessible ordering controls
- Add dates, notes, autosave, and favorites
- Enforce permissions and course ownership

### Phase 5: Aggregation and downloads — 2–3 weeks

- Consolidate chemical and equipment quantities
- Generate course schedules and attribution reports
- Generate PDFs and spreadsheets
- Package documents into ZIP archives
- Add download history and file expiration
- Introduce background generation if package size requires it

### Phase 6: Quality assurance and accessibility — 2 weeks

- Test business logic, permissions, and ownership
- Test REST API authorization and CSRF protection
- Test file uploads and generated packages
- Test large courses and duplicate resources
- Test search relevance
- Test keyboard and screen-reader use
- Check WCAG 2.2 AA color contrast and semantics
- Test responsive layouts and major browsers
- Perform backup and restoration testing

### Phase 7: Migration, launch, and training — 1–2 weeks

- Import and verify final content
- Configure redirects if replacing another site
- Write editor and administrator documentation
- Train the content team
- Configure analytics and monitoring
- Run a soft launch
- Complete production launch
- Provide a defined post-launch support period

## Security and operations

The implementation must include:

- Capability and ownership checks for every protected operation
- REST nonces and CSRF protection
- Input sanitization and output escaping
- Strict upload type and size limits
- Rate limiting for registrations and downloads
- Secure password-reset flows
- Minimal personal-data collection
- Automatic expiration of generated packages
- Audit logs for important administrative actions
- Daily database and file backups with off-site copies
- Staging and production environments
- Uptime, error, and email monitoring
- A documented and tested restoration procedure
- A maintenance process for WordPress, plugin, and server updates

## Accessibility

The target is WCAG 2.2 AA. Key requirements include:

- Complete keyboard operation
- Screen-reader labels and meaningful landmarks
- Accessible alternatives to drag-and-drop
- Sufficient color contrast
- Visible focus states
- Captions and transcripts for videos
- Accessible PDFs and downloadable documents
- Clear validation and error messages
- Responsive layouts that work under zoom

## Licensing and content governance

Experimentium states that much of its content uses a Creative Commons Attribution-NonCommercial-ShareAlike licence, while individual resources can have different terms. Any reused material must be checked individually before import.

Maintain a rights register containing:

- Resource name
- Author
- Source URL
- Licence
- Permitted modifications
- Required attribution
- Commercial-use restrictions
- Version or retrieval date

Content with a NoDerivatives licence must not be modified. The project's own branding, interface, code, and original educational content should be independently developed.

## Open questions

The following decisions are required before implementation estimates can be finalized:

1. Is the platform an open educational resource, a commercial service, or both?
2. Will all experiment details be public?
3. Is registration open, moderated, or invitation-only?
4. May instructors edit source documents or only assemble them?
5. Which PDF, DOCX, XLSX, and ZIP formats are required?
6. How should chemical and equipment quantities be calculated?
7. How many experiments and languages are expected at launch?
8. Will documents be created inside WordPress or uploaded from an external process?
9. Are student accounts or LMS integrations future requirements?
10. What reporting and analytics do project owners need?

## Next deliverables

Before development starts, prepare:

1. A Product Requirements Document with user stories and acceptance criteria.
2. A finalized experiment field schema and taxonomy.
3. Wireframes for the catalogue, experiment page, dashboard, and course builder.
4. A database and REST API specification.
5. A representative set of 5–10 fully prepared experiments for prototype testing.
6. A content licensing and attribution register.
7. A prioritized development backlog.

## Reference

- [Experimentium](https://www.experimentium.org/)
- [Experimentium teaching roadmap](https://www.experimentium.org/teach-with-experimentum/)
- [WordPress hosting requirements](https://wordpress.org/about/requirements/)
- [WordPress custom post types](https://developer.wordpress.org/plugins/post-types/registering-custom-post-types/)
- [WordPress roles and capabilities](https://developer.wordpress.org/plugins/users/roles-and-capabilities/)
- [WordPress REST API authentication](https://developer.wordpress.org/rest-api/using-the-rest-api/authentication/)
