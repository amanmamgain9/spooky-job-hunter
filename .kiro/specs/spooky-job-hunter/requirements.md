# Requirements Document

## Introduction

The Spooky Job Hunter is a Chrome extension that automates job searching across multiple platforms (LinkedIn, Wellfound, etc.) with a playful Halloween aesthetic. The system transforms the nightmare of job hunting into a whimsical, haunting experience through three main phases:

1. **Collect Job Criterion**: Upload CV, extract information, and define job preferences
2. **Search and Organize**: Automated multi-platform search with AI-powered relevance filtering
3. **Apply Using AI**: Intelligent form filling, application tracking, and submission management

## Glossary

- **Extension**: The Chrome browser extension application
- **Job Platform**: A website that hosts job listings (e.g., LinkedIn, Wellfound)
- **Platform Adapter**: A class that implements job search logic for a specific Job Platform
- **User Profile**: Collection of user information including CV, address, and other personal details
- **Cover Letter Generator**: AI-powered component that creates customized cover letters
- **React UI**: The user interface built with React framework
- **Spooky Theme**: Halloween-inspired visual design that is playful and beautiful, not scary or ugly

## Phase 1: Collect Job Criterion

### Requirement 1.1

**User Story:** As a job seeker, I want to upload and manage my CV and profile information within the extension, so that I can quickly apply to jobs with my current information.

#### Acceptance Criteria

1. WHEN a user uploads a CV file, THE Extension SHALL accept PDF, DOCX, and TXT file formats up to 5MB in size
2. WHEN a user uploads an unsupported file format, THE Extension SHALL reject the file and display an error message listing supported formats
3. WHEN a user enters profile information, THE Extension SHALL validate required fields (full name, email) before saving
4. WHEN profile data is saved, THE Extension SHALL persist the data across browser sessions using Chrome's storage API
5. THE Extension SHALL support profile fields including CV file, full name, email, phone number, street address, city, state/province, postal code, and country
6. WHEN a user updates profile information, THE Extension SHALL immediately reflect changes in the UI
7. WHEN CV file size exceeds 5MB, THE Extension SHALL reject the upload and inform the user of the size limit

### Requirement 1.2

**User Story:** As a job seeker, I want the extension to automatically extract information from my CV, so that I don't have to manually enter all my details.

#### Acceptance Criteria

1. WHEN a user uploads a CV file, THE Extension SHALL use AI to extract key information including skills, experience, education, and job titles
2. WHEN CV parsing completes, THE Extension SHALL populate profile fields with extracted information
3. WHEN extracted information is populated, THE Extension SHALL allow the user to review and edit the auto-filled data
4. WHEN CV parsing fails or produces low-confidence results, THE Extension SHALL notify the user and allow manual entry
5. THE Extension SHALL extract and store a list of skills and technologies mentioned in the CV for job matching

### Requirement 1.3

**User Story:** As a job seeker, I want to describe what kind of job I'm looking for in my own words, so that the extension can find relevant opportunities.

#### Acceptance Criteria

1. THE Extension SHALL provide a text input field for users to describe their job search criteria in natural language
2. WHEN a user enters job search criteria, THE Extension SHALL save the criteria text with the user profile
3. WHEN job search criteria text is provided, THE Extension SHALL use it in combination with CV information for job relevance filtering
4. THE Extension SHALL allow users to update their job search criteria at any time
5. WHEN no job search criteria is provided, THE Extension SHALL use only CV-extracted information for relevance filtering

## Phase 2: Search and Organize

### Requirement 2.1

**User Story:** As a job seeker, I want the extension to search for jobs automatically on multiple platforms, so that I can save time and discover opportunities across different websites.

#### Acceptance Criteria

1. WHEN a user clicks the search button, THE Extension SHALL open browser tabs for all enabled Job Platforms and navigate to their respective search pages
2. WHEN a user initiates a job search, THE Extension SHALL execute searches across all enabled Job Platforms simultaneously using content scripts injected into each platform's tab
3. WHEN a Job Platform returns search results, THE Extension SHALL parse the DOM structure using CSS selectors to extract job data (title, company, location, description, posting date) and display the job listings in a unified format
4. WHEN search parameters are modified, THE Extension SHALL update results from all Job Platforms by re-executing searches in the opened tabs
5. WHEN a Job Platform is unavailable or returns an error, THE Extension SHALL continue searching other platforms, display an error indicator for the failed platform, and log detailed error information
6. WHEN search results are retrieved, THE Extension SHALL deduplicate identical job postings across platforms by comparing job title, company name, and description similarity
7. WHEN navigating between pages on a Job Platform, THE Extension SHALL use MutationObserver or polling mechanisms to detect page load completion and element visibility before extracting data
8. WHEN DOM parsing fails due to website structure changes, THE Extension SHALL log the failed CSS selectors and page structure for debugging and notify the user that the platform may need updating

### Requirement 2.2

**User Story:** As a job seeker, I want the extension to automatically filter out irrelevant job listings, so that I only see opportunities that match my profile and preferences.

#### Acceptance Criteria

1. WHEN job results are retrieved from Job Platforms, THE Extension SHALL use AI (OpenAI API or compatible service) to analyze each job listing against the User Profile and job search criteria
2. WHEN analyzing job relevance, THE Extension SHALL send a prompt to the AI service containing the job description, CV-extracted skills, experience level, and job search criteria text, and receive a relevance score (0-100)
3. WHEN a job is determined to be irrelevant (score below configurable threshold, default 60), THE Extension SHALL exclude it from the displayed results by default
4. THE Extension SHALL assign a relevance score to each job listing and sort results by relevance in descending order
5. THE Extension SHALL allow users to toggle a "Show filtered jobs" option to view excluded jobs with their relevance scores
6. WHEN relevance filtering is performed, THE Extension SHALL complete the analysis within 10 seconds for up to 50 job listings by batching AI requests
7. WHEN a user provides feedback on a job's relevance (thumbs up/down), THE Extension SHALL store the feedback and use it to adjust future filtering thresholds

### Requirement 2.4

**User Story:** As a job seeker, I want to see job search results organized and filterable within the extension, so that I can quickly find relevant opportunities.

#### Acceptance Criteria

1. WHEN job results are displayed, THE Extension SHALL show job title, company, location, and source platform for each listing
2. WHEN a user applies filters, THE Extension SHALL update the displayed results to match the filter criteria
3. WHEN a user clicks on a job listing, THE Extension SHALL open the original job posting in a new tab
4. THE Extension SHALL support filtering by platform, location, job type, and posting date
5. WHEN no results match the current filters, THE Extension SHALL display a helpful message with suggestions

### Requirement 2.6

**User Story:** As a job seeker, I want the extension to remember my search preferences and history, so that I can resume my job search across sessions.

#### Acceptance Criteria

1. WHEN a user performs a search, THE Extension SHALL save the search parameters (keywords, location, platforms, filters) to Chrome's local storage API
2. WHEN the extension popup is reopened, THE Extension SHALL load the most recent search parameters and pre-populate the search form
3. THE Extension SHALL maintain a searchable history of previous searches with timestamps, limited to the most recent 50 searches
4. WHEN a user selects a previous search from history, THE Extension SHALL populate the search form with the saved parameters and optionally execute the search
5. WHEN a user clears their search history, THE Extension SHALL remove all stored search records from Chrome's local storage

### Requirement 2.7

**User Story:** As a user, I want to know which job platforms are currently supported and their status, so that I can understand where my searches are being performed.

#### Acceptance Criteria

1. THE Extension SHALL display a list of all supported Job Platforms (LinkedIn, Wellfound) in the settings interface
2. WHEN displaying Job Platforms, THE Extension SHALL show the platform name, logo, and current status (enabled/disabled/error)
3. WHEN a user toggles a Job Platform, THE Extension SHALL enable or disable searches for that platform and update the setting in Chrome's local storage
4. THE Extension SHALL persist platform enable/disable preferences across browser sessions using Chrome's storage API
5. WHEN a platform encounters repeated errors (3 or more consecutive failures), THE Extension SHALL automatically disable that platform, display an error badge, and show a notification to the user

## Phase 3: Apply Using AI

### Requirement 3.1

**User Story:** As a job seeker, I want to easily navigate from search results to applying for a job, so that I can quickly take action on opportunities.

#### Acceptance Criteria

1. WHEN viewing a job listing in search results, THE Extension SHALL provide an "Apply" button or action for each job
2. WHEN a user clicks "Apply" on a job listing, THE Extension SHALL use Chrome's tabs API to navigate to the job's application page in a new tab
3. WHEN the application page loads, THE Extension SHALL inject a content script that automatically detects if it is an application form page by searching for form elements with common application field patterns (name, email, resume upload, etc.)
4. WHEN an application form is detected, THE Extension SHALL display a notification badge or popup informing the user that auto-fill is available
5. THE Extension SHALL provide a way to initiate the application process directly from the job details view within the popup interface

### Requirement 3.2

**User Story:** As a job seeker, I want the extension to automatically generate customized cover letters for job applications, so that I can apply to more positions with personalized content.

#### Acceptance Criteria

1. WHEN a user requests cover letter generation for a job, THE Extension SHALL send a prompt to the AI service (OpenAI API or compatible) containing the job description, job title, company name, and User Profile information (CV summary, skills, experience) to create a customized cover letter
2. WHEN the Cover Letter Generator produces output, THE Extension SHALL present the text in an editable textarea within the popup interface
3. WHEN generating a cover letter, THE Extension SHALL complete the generation within 30 seconds or display a timeout error
4. WHEN cover letter generation fails due to API errors, THE Extension SHALL display a user-friendly error message with the specific error reason and provide a "Retry" button
5. WHEN a cover letter is generated, THE Extension SHALL provide options to copy to clipboard, save to Chrome's local storage for future reference, or directly paste into an application form

### Requirement 3.3

**User Story:** As a job seeker, I want the cover letter generator to use specific AI models or services, so that I receive high-quality, contextual cover letters.

#### Acceptance Criteria

1. THE Cover Letter Generator SHALL support integration with OpenAI API or compatible services by making HTTP requests to the configured API endpoint
2. WHEN generating a cover letter, THE Cover Letter Generator SHALL construct a prompt that includes job title, company name, job description, CV-extracted skills, experience summary, and job search criteria
3. THE Extension SHALL allow users to configure their AI API key and optionally the API endpoint URL in the settings interface
4. WHEN no API key is configured, THE Extension SHALL display a setup prompt with step-by-step instructions for obtaining an API key and a link to the provider's website
5. THE Cover Letter Generator SHALL include a system prompt instructing the AI to limit generated cover letters to 500 words maximum and maintain a professional yet personalized tone

### Requirement 3.4

**User Story:** As a job seeker, I want the extension to automatically fill out job application forms, so that I can apply to jobs quickly without repetitive data entry.

#### Acceptance Criteria

1. WHEN a user is on a job application page, THE Extension SHALL inject a content script that detects application form fields by analyzing input elements, their labels, placeholders, and name/id attributes
2. WHEN application form fields are detected, THE Extension SHALL automatically populate fields with User Profile data by matching field identifiers to profile fields (name, email, phone, street address, city, state, postal code, country)
3. WHEN a form field matches a CV field or extracted skill, THE Extension SHALL fill it with the corresponding value using JavaScript to set the input value and trigger appropriate change events
4. WHEN the Extension fills form fields, THE Extension SHALL add a visual indicator (colored border or background) to highlight filled fields for user review
5. THE Extension SHALL support common form field types including text inputs, textareas, dropdowns (select elements), checkboxes, radio buttons, and file upload inputs
6. WHEN a CV upload field is detected (input type="file" with accept attributes for documents), THE Extension SHALL programmatically attach the user's stored CV file by creating a File object from the stored data
7. WHEN filling dropdown fields, THE Extension SHALL match User Profile values to dropdown options using fuzzy matching to handle variations in option text

### Requirement 3.5

**User Story:** As a job seeker, I want to preview and review my application before submission, so that I can ensure all information is correct.

#### Acceptance Criteria

1. WHEN application form fields are auto-filled, THE Extension SHALL display a side panel or overlay showing all filled information organized by field name and value
2. THE Extension SHALL scan for required fields (marked with asterisks, "required" attribute, or aria-required) and highlight any that remain empty with a warning indicator
3. WHEN reviewing the application in the preview panel, THE Extension SHALL allow the user to edit any auto-filled values by clicking on them, which updates both the preview and the actual form field
4. THE Extension SHALL provide a clear "Submit Application" button in the preview panel that requires explicit user click approval before submission
5. WHEN the user clicks the submit button, THE Extension SHALL programmatically trigger the form submission by clicking the platform's submit button or calling form.submit(), then save the application record

### Requirement 3.6

**User Story:** As a job seeker, I want to track all my job applications in one place, so that I can monitor my application history and status.

#### Acceptance Criteria

1. WHEN an application is submitted, THE Extension SHALL save a record to Chrome's local storage including job title, company, platform, job URL, submission date, application status (submitted/pending/rejected/interview), and a snapshot of all submitted form data
2. THE Extension SHALL provide a dedicated "Application History" view in the popup showing all submitted applications in a list or table format with their current status
3. WHEN viewing application history, THE Extension SHALL allow filtering by date range, platform, and status using dropdown filters and a search box
4. WHEN a user clicks on an application record, THE Extension SHALL display full application details in an expanded view including job description, submitted answers, cover letter used, and submission timestamp
5. THE Extension SHALL allow users to add and edit notes to application records, with notes saved to Chrome's local storage and displayed in the application details view
6. WHEN a user views a job listing in search results, THE Extension SHALL check if the job URL or job title+company combination matches an existing application record and display a clear "Already Applied" badge
7. WHEN a user attempts to apply to a job they have already applied to, THE Extension SHALL display a warning modal showing the previous application date and details, with options to proceed anyway or cancel

### Requirement 3.7

**User Story:** As a job seeker, I want the extension to remember my answers to custom application questions, so that I don't have to retype similar answers repeatedly.

#### Acceptance Criteria

1. WHEN a custom application question is encountered (detected by analyzing form field labels and question text), THE Extension SHALL query the stored question database to check if a similar question has been answered before using text similarity comparison
2. WHEN a similar question is found (similarity score above 80%), THE Extension SHALL display the previous answer in a suggestion tooltip or auto-fill the field with a visual indicator that it's a suggested answer
3. WHEN a user answers a custom question, THE Extension SHALL save the question text, answer, and timestamp to Chrome's local storage for future use
4. THE Extension SHALL use AI (OpenAI embeddings or text similarity API) to match similar questions even when wording differs by comparing semantic meaning rather than exact text
5. WHEN suggesting a previous answer, THE Extension SHALL display edit and reject buttons allowing the user to modify the suggested answer or clear it to enter a new one
6. THE Extension SHALL maintain a searchable database of all answered custom questions stored in Chrome's local storage, with a management interface in settings to view, edit, or delete saved questions

### Requirement 3.8

**User Story:** As a job seeker, I want the extension to handle CAPTCHAs appropriately, so that I can complete applications that include CAPTCHA verification.

#### Acceptance Criteria

1. WHEN a CAPTCHA is detected on an application form (by searching for common CAPTCHA iframe elements, reCAPTCHA divs, or hCaptcha elements), THE Extension SHALL pause auto-fill and display a notification to the user
2. THE Extension SHALL highlight the CAPTCHA element with a pulsing border or overlay to draw user attention
3. WHEN the user completes the CAPTCHA (detected by monitoring for CAPTCHA completion events or token presence), THE Extension SHALL automatically resume the application process
4. THE Extension SHALL not attempt to bypass or automate CAPTCHA solving in any way

### Requirement 3.9

**User Story:** As a job seeker, I want the extension to handle authentication requirements, so that I can apply to jobs on platforms where I'm not logged in.

#### Acceptance Criteria

1. WHEN a job application requires authentication, THE Extension SHALL detect the login requirement by checking for login forms, authentication redirects, or "sign in" buttons on the page
2. WHEN authentication is required, THE Extension SHALL pause the application process and display a notification banner to the user
3. THE Extension SHALL display a message instructing the user to log in to the platform, with a button to open the login page if not already there
4. WHEN the user completes authentication (detected by monitoring URL changes or the appearance of authenticated user elements), THE Extension SHALL automatically resume the application process
5. THE Extension SHALL not store or manage user credentials for job platforms in any way

## Cross-Cutting Requirements

### Requirement 4.1

**User Story:** As a job seeker, I want the extension to have a playful, beautiful Halloween theme, so that the stressful job search process feels more whimsical and less overwhelming.

#### Acceptance Criteria

1. THE React UI SHALL use a color palette inspired by Halloween (purples #6B46C1, oranges #F97316, blacks #1F2937) with high contrast ratios (minimum 4.5:1) for readability
2. THE React UI SHALL include playful Halloween-themed iconography (friendly ghosts, pumpkins, bats) as SVG icons throughout the interface
3. THE React UI SHALL use smooth CSS animations and transitions (0.3s ease-in-out) that evoke a haunting but pleasant atmosphere
4. THE React UI SHALL maintain professional readability by using clear typography (minimum 14px font size) while incorporating thematic elements
5. WHEN users interact with UI elements (buttons, cards, inputs), THE React UI SHALL provide subtle spooky-themed feedback using CSS transitions for glow effects, scale transforms, and floating animations

### Requirement 4.2

**User Story:** As a user, I want the extension to provide a React-based popup interface, so that I can interact with all features through a modern, responsive UI.

#### Acceptance Criteria

1. WHEN a user clicks the extension icon, THE Extension SHALL display a React UI popup within 1 second by loading the pre-built React bundle
2. THE React UI SHALL provide navigation between job search, profile management, application history, and settings sections using a tab-based or sidebar navigation component
3. THE React UI SHALL display real-time status updates during job searches by receiving messages from the background service worker and updating the UI state
4. THE React UI SHALL be responsive and functional at the standard Chrome extension popup dimensions (minimum 400px width, 600px height) using responsive CSS
5. WHEN the popup is closed and reopened, THE React UI SHALL restore the user's previous view state (active tab, scroll position) by saving state to Chrome's session storage

### Requirement 4.3

**User Story:** As a user, I want the extension to work reliably on Chrome browser, so that I can use it on my primary browser.

#### Acceptance Criteria

1. THE Extension SHALL be compatible with Chrome browser version 100 and above by using only APIs available in those versions
2. THE Extension SHALL use Manifest V3 for Chrome extension architecture with a manifest.json file specifying manifest_version: 3
3. WHEN the Extension is installed, THE Extension SHALL request only necessary permissions in the manifest.json: storage (for saving data), activeTab (for accessing current tab), scripting (for injecting content scripts), and tabs (for managing tabs)

### Requirement 4.4

**User Story:** As a user, I want the extension to handle errors gracefully with themed messaging, so that technical issues don't break the experience.

#### Acceptance Criteria

1. WHEN an error occurs, THE Extension SHALL display a Halloween-themed error message (with spooky icons and themed colors) that explains the issue in user-friendly language
2. WHEN a platform search fails, THE Extension SHALL continue operating with remaining platforms and display a partial results message indicating which platform failed
3. WHEN network connectivity is lost (detected by failed fetch requests or navigator.onLine), THE Extension SHALL detect the condition and display a notification with a "Retry" button
4. WHEN the Cover Letter Generator encounters an error, THE Extension SHALL preserve the user's input in the form fields and display an error message with a "Retry" button
5. THE Extension SHALL log all errors to the browser console using console.error() with detailed stack traces for debugging purposes while showing user-friendly messages in the UI

