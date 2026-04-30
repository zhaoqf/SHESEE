## MODIFIED Requirements

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

### Requirement: Website URL display
The system SHALL display the website URL as plain text in the table cell.

#### Scenario: URL displayed as text
- **WHEN** school data is rendered in the table
- **THEN** the website field SHALL be displayed as plain text
- **AND** not as a clickable link
- **AND** the full URL SHALL be visible and selectable
