# Design Document

## Overview

The Student Lecture Tracker is a single-page React application built with Tailwind CSS that provides a responsive interface for managing student profiles, lecture schedules, and lecture notes. The application uses React hooks for state management and the browser's Local Storage API for data persistence. The architecture follows a component-based design pattern with clear separation between UI components, state management, and storage utilities.

## Architecture

The application follows a simple Flask MVC architecture:

1. **Presentation Layer**: Jinja2 templates with glassmorphism CSS styling
2. **Controller Layer**: Flask routes handling HTTP requests
3. **Storage Layer**: Browser Local Storage via JavaScript for data persistence
4. **Data Layer**: Python dictionaries and JSON for data models

The application uses server-side rendering with client-side JavaScript for interactivity and Local Storage persistence.

## Components and Interfaces

### Core Components

#### App Component
- Root component managing global application state
- Coordinates profile data and lecture list
- Handles Local Storage initialization and synchronization

#### ProfileSection Component
- Displays and manages user profile information
- Props: `profile`, `onProfileUpdate`
- Emits profile changes to parent component

#### LectureList Component
- Renders the collection of lecture cards
- Props: `lectures`, `onLectureUpdate`, `onLectureDelete`
- Manages lecture addition through an add lecture form

#### LectureCard Component
- Displays individual lecture information
- Props: `lecture`, `onUpdate`, `onDelete`
- Contains embedded NoteSection component
- Provides edit and delete functionality

#### NoteSection Component
- Manages note-taking for a specific lecture
- Props: `lectureId`, `initialNote`, `onNoteSave`
- Contains Save and Clear buttons
- Handles local note state before persistence

#### AddLectureForm Component
- Form for creating new lectures
- Props: `onAdd`
- Validates subject and time inputs
- Emits new lecture data to parent

### Component Hierarchy

```
App
├── ProfileSection
├── LectureList
│   ├── AddLectureForm
│   └── LectureCard (multiple)
│       ├── LectureInfo
│       └── NoteSection
```

## Data Models

### Profile Interface
```typescript
interface Profile {
  name: string;
  collegeName: string;
}
```

### Lecture Interface
```typescript
interface Lecture {
  id: string;
  subject: string;
  time: string;
  note: string;
}
```

### AppState Interface
```typescript
interface AppState {
  profile: Profile;
  lectures: Lecture[];
}
```

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*


### Property 1: Profile data round-trip persistence
*For any* valid profile data (name and college name), saving the profile and reloading the application should restore the exact same profile information.
**Validates: Requirements 1.3, 1.4**

### Property 2: Profile validation rejects empty inputs
*For any* string composed entirely of whitespace or empty strings, attempting to save a profile with such values should be rejected and the profile should remain unchanged.
**Validates: Requirements 1.2**

### Property 3: Profile updates persist immediately
*For any* valid profile and any valid edit to that profile, saving the changes should immediately update both the displayed profile and the Local Storage data.
**Validates: Requirements 1.5**

### Property 4: Lecture data round-trip persistence
*For any* set of valid lectures (including their notes), saving the lectures and reloading the application should restore all lectures with their exact subject, time, and note content.
**Validates: Requirements 2.4, 2.5, 3.5**

### Property 5: Adding valid lecture grows the list
*For any* lecture list and any valid lecture (non-empty subject and time), adding the lecture should increase the list length by one and the new lecture should appear in both the display and Local Storage.
**Validates: Requirements 2.2**

### Property 6: Lecture validation rejects invalid inputs
*For any* lecture with empty or whitespace-only subject or time fields, attempting to add the lecture should be rejected, the list should remain unchanged, and validation feedback should be displayed.
**Validates: Requirements 2.3**

### Property 7: Note save persists to storage
*For any* lecture and any note content, clicking the Save button should persist the note to Local Storage and the saved note should be retrievable on reload.
**Validates: Requirements 3.3**

### Property 8: Note clear removes content
*For any* lecture with any note content, clicking the Clear button should empty the note text area and update Local Storage to reflect the empty note.
**Validates: Requirements 3.4**

### Property 9: Lecture edit updates display and storage
*For any* lecture and any valid edits to its subject or time, saving the edits should update the lecture card display and persist the changes to Local Storage.
**Validates: Requirements 4.2**

### Property 10: Lecture deletion removes from list and storage
*For any* lecture list and any lecture in that list, deleting the lecture should remove it from the displayed list, remove it from Local Storage, and remove its associated note data.
**Validates: Requirements 4.3, 4.4**

### Property 11: Responsive layout adapts to viewport
*For any* viewport width between 320px and 1920px, all interactive elements should remain accessible, text should remain readable, and lecture cards should reflow to fit the available space without horizontal scrolling.
**Validates: Requirements 5.1, 5.3, 5.4**

### Property 12: Touch targets meet minimum size
*For any* interactive element (button, input, text area), the element should have a minimum touch target size of 44x44 pixels to ensure touch-friendly interaction on mobile devices.
**Validates: Requirements 5.2**

### Property 13: Interactive feedback is immediate
*For any* user interaction with buttons or inputs, visual feedback (hover state, focus state, or active state) should be provided within the same render cycle.
**Validates: Requirements 6.3**

## Error Handling

### Input Validation Errors
- Empty or whitespace-only profile fields should display inline validation messages
- Empty or whitespace-only lecture fields should prevent form submission and show error messages
- Validation should occur on blur and on submit

### Storage Errors
- If Local Storage is unavailable (private browsing, storage quota exceeded), the application should display a warning message and continue functioning with in-memory state only
- If Local Storage contains corrupted data (invalid JSON), the application should log the error, clear the corrupted data, and initialize with empty state
- All storage operations should be wrapped in try-catch blocks

### Edge Cases
- Empty state: Application should display helpful empty state messages when no profile or lectures exist
- Maximum storage: If Local Storage quota is exceeded, notify the user and suggest deleting old lectures
- Concurrent tabs: Changes in one tab may not reflect in another tab (acceptable limitation, or implement storage event listeners for synchronization)

## Testing Strategy

### Unit Testing
The application will use Vitest as the testing framework with React Testing Library for component testing.

**Unit Test Coverage:**
- Profile validation logic (empty string detection, whitespace handling)
- Lecture validation logic (required fields, format validation)
- Storage utility functions (serialize, deserialize, error handling)
- Component rendering (ProfileSection, LectureCard, NoteSection)
- User interactions (button clicks, form submissions, text input)
- Edge cases (empty state, corrupted storage data, storage unavailable)

### Property-Based Testing
The application will use fast-check for property-based testing to verify universal properties across many randomly generated inputs.

**Property Test Configuration:**
- Each property test should run a minimum of 100 iterations
- Each property test must include a comment tag referencing the correctness property from this design document
- Tag format: `// Feature: student-lecture-tracker, Property {number}: {property_text}`

**Property Test Coverage:**
- Property 1: Profile round-trip with random profile data
- Property 2: Profile validation with random invalid inputs
- Property 3: Profile updates with random edits
- Property 4: Lecture round-trip with random lecture sets
- Property 5: Adding lectures with random valid data
- Property 6: Lecture validation with random invalid inputs
- Property 7: Note persistence with random note content
- Property 8: Note clearing with random initial content
- Property 9: Lecture editing with random edits
- Property 10: Lecture deletion with random lecture lists
- Property 11: Responsive layout at random viewport widths
- Property 12: Touch target sizes for all interactive elements
- Property 13: Interactive feedback timing

### Integration Testing
- End-to-end user flows: Create profile → Add lectures → Take notes → Edit lectures → Delete lectures
- Storage integration: Verify all CRUD operations correctly update Local Storage
- Cross-component communication: Verify state updates propagate correctly through component tree

## Implementation Details

### Technology Stack
- **Python 3.8+**: Backend language
- **Flask**: Lightweight web framework
- **Jinja2**: Template engine (built into Flask)
- **CSS3**: Custom glassmorphism styling
- **JavaScript (Vanilla)**: Client-side interactivity
- **Port 8000**: Application server port
- **Virtual Environment**: Isolated Python dependencies

### State Management
- Use `useState` for component-local state
- Use `useEffect` for Local Storage synchronization
- Lift state to App component for shared data (profile, lectures)
- Pass state and updater functions via props

### Local Storage Schema
```typescript
// Storage key: 'student-lecture-tracker-data'
{
  profile: {
    name: string,
    collegeName: string
  },
  lectures: [
    {
      id: string,  // UUID v4
      subject: string,
      time: string,  // Format: "HH:MM AM/PM" or "HH:MM"
      note: string
    }
  ]
}
```

### Responsive Breakpoints
- Mobile: < 640px (single column)
- Tablet: 640px - 1024px (two columns)
- Desktop: > 1024px (three columns or grid layout)

### Styling Guidelines - Glassmorphism
- Background: Semi-transparent with backdrop-filter blur
- Cards: rgba(255, 255, 255, 0.1) with backdrop-filter: blur(10px)
- Borders: 1px solid rgba(255, 255, 255, 0.2)
- Shadows: 0 8px 32px rgba(0, 0, 0, 0.1)
- Border radius: 16px for cards
- Gradient background: Linear gradient with vibrant colors
- Text: White or light colors for contrast on glass elements
- Buttons: Glass effect with hover brightness increase

### Accessibility Considerations
- All form inputs should have associated labels
- Buttons should have descriptive text or aria-labels
- Color should not be the only means of conveying information
- Focus states should be clearly visible
- Semantic HTML elements should be used (header, main, section, article)
