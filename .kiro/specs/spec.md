# Project Spec: Spelling Test App MVP

## 1. Product Summary
This project is a web-based spelling test application intended primarily for use on a Windows 11 SE laptop. A parent or teacher sets up the test, and then a single child takes the spelling test one word at a time.

The MVP should provide a strong foundation for future features while staying simple enough to build quickly. The app should allow an adult to import a spelling list from a CSV file, save word lists locally, administer a spelling test one word at a time, provide pronunciation audio automatically for each word, allow the child to replay the word on demand, collect typed answers, and show a final results screen with score, all answers, and missed words.

The MVP should not require any backend server, cloud account, or hosted database. All saved data should be stored locally in the browser.

## 2. Primary Users

### Primary user
- A single child taking a spelling test

### Secondary user
- A parent or teacher who prepares and starts the test

### User characteristics
- Child UI should target an elementary-age reading level
- Child-facing screens should be simple and focused
- Setup/admin controls should be separated from the child’s main test-taking experience

### Usage environment
- Main target device is a Windows 11 SE laptop
- The app should work well in a home or school setting
- The interface should work well with keyboard input

## 3. MVP Goals
The MVP must support the following:
1. Import a spelling list from a CSV file
2. Save word lists locally
3. Administer a spelling test one word at a time
4. Automatically pronounce each word when it is shown
5. Let the child replay the word with a Repeat Word button
6. Let the child type an answer for each word
7. Allow skipping words
8. Allow returning to previous words
9. Show a final results screen with total score, all answers, and missed words
10. Save full test history locally

### MVP emphasis
- Strong foundation for future features
- Simple child experience
- Local-first browser storage
- No backend complexity

## 4. Non-Goals
The MVP must not include:
- User accounts or login
- Cloud sync
- Multi-student profiles
- Speech recognition
- Fancy animations or elaborate themes
- Any backend, server, or hosted database
- Online multiplayer or real-time shared use

Teacher dashboard/reporting features are out of scope for MVP unless explicitly added later.

## 5. Core User Flows

### Flow 1: Import and create a test
1. The adult opens the app
2. The adult chooses to create a new spelling test
3. The adult imports a CSV file containing spelling words and sentences
4. The app parses the file into a word list
5. The app saves the word list locally
6. The adult reviews the imported list and starts the test

### Flow 2: Take the spelling test
1. The child starts the test
2. The app presents one word at a time
3. When the word is shown, the app automatically pronounces it
4. The app shows the sentence for the current word, with the spelling word blanked out if possible
5. The child can press a Repeat Word button to hear the word again
6. The child types the spelling into a text input
7. After submission, the app immediately advances to the next word
8. The child can move back to previous words
9. The child can skip a word
10. Skipped words remain in position and are shown as unanswered
11. The app continues until the test is complete

### Flow 3: Finish and view results
1. When the test is complete, the app shows a results screen
2. The results screen includes:
   - overall score
   - each spelling word
   - the child’s submitted answer for each word
   - which words were incorrect
3. The completed test is saved in local test history

## 6. Functional Requirements

### 6.1 Importing word lists
- Support CSV import only
- CSV must contain exactly two columns:
  - `Word`
  - `Sentence`
- Each row represents one spelling entry
- Ignore completely blank rows
- Preserve duplicate words
- Maximum 50 words per imported list
- If required columns are missing, show a clear import error
- If the file contains more than 50 rows, show a clear error and do not import it

### 6.2 Test behavior
- Present one word at a time
- Automatically pronounce the current word when it is first shown
- Provide a visible Repeat Word button that replays the pronunciation
- Allow typed answers
- Save progress after each answer
- Allow back navigation to previous words
- Allow skipping words
- Keep skipped words in their original position and mark them unanswered
- Show visible progress such as “Word 4 of 12”
- Do not reveal correctness until the end

### 6.3 Sentence support
- Display the sentence associated with the current word
- If possible, replace the target spelling word in the sentence with a blank
- If automatic blanking is not reliable, display the sentence as stored in the CSV for MVP
- Structure the code so sentence display logic can be improved later without a major rewrite

### 6.4 Audio
- Use browser text-to-speech for MVP
- Automatically speak the word when the question appears
- Allow the child to replay the word by pressing a Repeat Word button
- Structure the code so a future enhancement could replace or supplement this with downloaded/cached audio from a web service

### 6.5 Scoring
- Trim leading/trailing spaces before grading
- Require exact capitalization match
- Require exact punctuation match
- Score answers strictly after trimming outside whitespace

### 6.6 Results
- Show total score
- Show all correct words
- Show submitted answers
- Show missed words

### 6.7 History
- Save full test history locally
- Each history entry must include:
  - date
  - score
  - missed words

### 6.8 Recovery
- Restore an in-progress test if the browser tab/app closes and is reopened

## 7. UX / UI Requirements
- Child-facing screen should be very minimal
- Prioritize one clear main input and large buttons
- Avoid clutter and unnecessary controls
- Keep admin/setup controls separate from the test-taking UI
- Show progress during the test
- Do not show correctness during the test
- Make the Repeat Word button easy to find and use
- Optimize for keyboard-friendly use on Windows 11 SE laptop

## 8. PWA / Offline Requirements
- Offline support is not required for MVP
- Installability as a PWA is not required for MVP
- The code should still be structured so PWA support can be added later if desired

## 9. Technical Constraints
- Preferred stack: React + TypeScript
- This preference can be changed if there is a strong reason to simplify
- No backend
- No hosted database
- Store all persistent data locally in browser storage
- Preferred storage for MVP: localStorage
- Keep architecture simple and easy to extend later

## 10. Data Model

### WordList
- id
- name
- entries[]
- createdAt

### WordListEntry
- word
- sentence

### TestSession
- id
- wordListId
- currentIndex
- answers[]
- skippedWordIndexes[]
- startedAt
- updatedAt

### TestHistoryEntry
- id
- wordListId
- date
- score
- missedWords[]

## 11. Acceptance Criteria
The MVP is complete when:
1. CSV import works with exactly two columns: Word and Sentence
2. Blank rows are ignored
3. Duplicate words are preserved
4. Lists up to 50 rows work reliably
5. A child can complete a spelling test one word at a time
6. Each word is automatically spoken when shown
7. The Repeat Word button works
8. Typed answer entry works
9. Progress is saved after each answer
10. Skipping works
11. Returning to previous words works
12. Progress indicator is visible
13. Correctness is hidden until the end
14. Trimming of accidental outside spaces works
15. Capitalization and punctuation are graded strictly
16. Results screen shows score, all answers, and missed words
17. Test history is saved locally
18. In-progress tests can be restored
19. The app works reliably for one child on one device
20. Basic tests are included
21. The codebase is clear enough to extend later