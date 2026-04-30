## ADDED Requirements

### Requirement: Page loads data from JSON files on initialization
The system SHALL fetch all three JSON data files from `data/2025/` directory when the page DOM is ready.

#### Scenario: All JSON files load successfully
- **WHEN** page DOM is ready and all JSON files are accessible
- **THEN** system SHALL parse the JSON data and render schools and timeline

#### Scenario: Some JSON files fail to load
- **WHEN** one or more JSON files cannot be fetched
- **THEN** system SHALL display an error message to the user
- **AND** system SHALL NOT render partial or stale data

### Requirement: City schools data structure
The system SHALL expect `city_schools.json` to contain an array of city school objects.

#### Scenario: Valid city schools data
- **WHEN** `city_schools.json` is fetched successfully
- **THEN** each object in the array SHALL contain `name` (string), `plan` (number), `website` (string) fields

### Requirement: District schools data structure
The system SHALL expect `district_schools.json` to contain an array of district school objects.

#### Scenario: Valid district schools data
- **WHEN** `district_schools.json` is fetched successfully
- **THEN** each object in the array SHALL contain `district` (string), `name` (string), `plan` (number), `website` (string) fields

### Requirement: Timeline data structure
The system SHALL expect `timeline.json` to contain an array of timeline event objects.

#### Scenario: Valid timeline data
- **WHEN** `timeline.json` is fetched successfully
- **THEN** each object in the array SHALL contain `date` (string) and `content` (string) fields
