## ADDED Requirements

### Requirement: Edit button visible for each school record
The system SHALL display an edit button for each school row in both city and district school tables.

#### Scenario: Edit button displayed
- **WHEN** school data is loaded and rendered in the table
- **THEN** each row SHALL have an "编辑" button in the last column

### Requirement: Edit modal opens with current school data
The system SHALL open an edit modal populated with the selected school's current data when the user clicks the edit button.

#### Scenario: Modal opens with correct data
- **WHEN** user clicks the edit button for a school row
- **THEN** a modal SHALL open with fields pre-filled: name, plan, website
- **AND** for district schools, the district field SHALL also be pre-filled

### Requirement: Editable fields
The system SHALL allow the user to modify the following fields:
- 学校名称 (name): text input
- 培养计划 (plan): number input
- 官方网站 (website): text input
- 区 (district): dropdown select (district schools only)

#### Scenario: All fields are editable
- **WHEN** edit modal is open
- **THEN** user SHALL be able to modify name, plan, website fields
- **AND** for district schools, user SHALL also be able to modify district

### Requirement: Save writes to JSON file
The system SHALL write the updated school record to the corresponding JSON file when the user clicks the save button.

#### Scenario: Successful save
- **WHEN** user clicks "保存" button with modified data
- **AND** user has previously authorized file access
- **THEN** the system SHALL update the school record in the JSON file
- **AND** close the modal

#### Scenario: First save - file authorization
- **WHEN** user clicks "保存" button for the first time
- **THEN** browser SHALL prompt user to select the target JSON file
- **AND** after authorization, the save SHALL proceed

#### Scenario: Save failure
- **WHEN** user clicks "保存" but file write fails
- **THEN** system SHALL display an error message
- **AND** modal SHALL remain open for retry

### Requirement: Cancel closes modal without saving
The system SHALL close the edit modal without modifying any data when the user clicks "取消".

#### Scenario: Cancel closes modal
- **WHEN** user clicks "取消" button
- **THEN** modal SHALL close
- **AND** no changes SHALL be written to the JSON file
