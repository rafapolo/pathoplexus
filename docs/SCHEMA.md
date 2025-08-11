# Database Schema Documentation

The Pathoplexus database models are defined using Kotlin's Exposed framework. Here are the key database models:

## Database Schema Diagram

```
┌─────────────────────────────────────┐
│              GROUPS_TABLE           │
├─────────────────────────────────────┤
│ PK: group_id (INT)                  │
│     group_name                      │
│     institution                     │
│     address_line_1                  │
│     address_line_2                  │
│     address_postal_code             │
│     address_city                    │
│     address_state                   │
│     address_country                 │
│     contact_email                   │
└─────────────────────────────────────┘
                    │
                    │ 1:N
                    ▼
┌─────────────────────────────────────┐
│           USER_GROUPS_TABLE         │
├─────────────────────────────────────┤
│ PK: id (INT)                        │
│     user_name                       │
│ FK: group_id → GROUPS_TABLE         │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│         SEQUENCE_ENTRIES            │
├─────────────────────────────────────┤
│ PK: accession, version              │
│     original_data (JSONB)           │
│     organism                        │
│     submission_id                   │
│     submitter                       │
│     approver                        │
│ FK: group_id → GROUPS_TABLE         │
│     submitted_at                    │
│     released_at                     │
│     is_revocation                   │
│     version_comment                 │
└─────────────────────────────────────┘
                    │
                    │ 1:N
                    ▼
┌─────────────────────────────────────┐
│      EXTERNAL_METADATA_TABLE        │
├─────────────────────────────────────┤
│ PK: accession, version, updater_id  │
│     external_metadata (JSONB)       │
│     updated_metadata_at             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│   SEQUENCE_ENTRIES_PREPROCESSED_    │
│           DATA_TABLE                │
├─────────────────────────────────────┤
│ PK: accession, version              │
│     processed_data (JSONB)          │
│     pipeline_version                │
│     started_processing_at           │
│     finished_processing_at          │
│     errors_warnings (JSONB)         │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│        DATA_USE_TERMS_TABLE         │
├─────────────────────────────────────┤
│     accession                       │
│     change_date                     │
│     data_use_terms_type             │
│     restricted_until                │
│     user_name                       │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│            FILES_TABLE              │
├─────────────────────────────────────┤
│ PK: id (UUID)                       │
│     upload_requested_at             │
│     uploader                        │
│ FK: group_id → GROUPS_TABLE         │
│     released_at                     │
│     size                            │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│           SEQSETS_TABLE             │
├─────────────────────────────────────┤
│ PK: seqset_id, seqset_version       │
│     name                            │
│     description                     │
│     seqset_doi                      │
│     created_at                      │
│     created_by                      │
└─────────────────────────────────────┘
                    │
                    │ M:N
                    ▼
┌─────────────────────────────────────┐
│       SEQSET_TO_RECORDS_TABLE       │
├─────────────────────────────────────┤
│ PK: seqset_record_id, seqset_id,    │
│     seqset_version                  │
│ FK: seqset_record_id → SEQSET_      │
│     RECORDS_TABLE                   │
│ FK: seqset_id, seqset_version →     │
│     SEQSETS_TABLE                   │
└─────────────────────────────────────┘
                    │
                    │ N:1
                    ▼
┌─────────────────────────────────────┐
│        SEQSET_RECORDS_TABLE         │
├─────────────────────────────────────┤
│ PK: seqset_record_id (AUTO)         │
│     accession                       │
│     type                            │
│     is_focal                        │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│     CURRENT_PROCESSING_PIPELINE     │
├─────────────────────────────────────┤
│ PK: organism                        │
│     pipeline_version                │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│        UPDATE_TRACKER_TABLE         │
├─────────────────────────────────────┤
│ PK: table_name                      │
│     last_time_updated_db            │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│          AUDIT_LOG_TABLE            │
├─────────────────────────────────────┤
│ PK: id (AUTO)                       │
│     timestamp                       │
│     user_name                       │
│     action                          │
│     table_name                      │
│     old_values (JSONB)              │
│     new_values (JSONB)              │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│      METADATA_UPLOAD_AUX_TABLE      │
├─────────────────────────────────────┤
│     upload_id                       │
│     submission_id                   │
│     group_id                        │
│     organism                        │
│     original_metadata (JSONB)       │
│     uploaded_at                     │
│     submitter                       │
│     files_mapping (JSONB)           │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│      SEQUENCE_UPLOAD_AUX_TABLE      │
├─────────────────────────────────────┤
│     upload_id                       │
│     sequence_submission_id          │
│     organism                        │
│     segment_name                    │
│     unaligned_sequence              │
└─────────────────────────────────────┘
```

## Core Submission Models

**SequenceEntriesTable** (`backend/src/main/kotlin/org/loculus/backend/service/submission/SequenceEntriesTable.kt:19`)
- Primary table for sequence submissions
- Fields: accession, version, organism, submissionId, submitter, approver, groupId, timestamps
- Primary key: (accession, version)
- Contains original_data as JSONB for sequence data

**ExternalMetadataTable** (`backend/src/main/kotlin/org/loculus/backend/service/submission/ExternalMetadataTable.kt:10`)
- Stores external metadata updates
- Primary key: (accession, version, updater_id)
- Contains external_metadata as JSONB

## User & Group Management

**GroupsTable** (`backend/src/main/kotlin/org/loculus/backend/service/groupmanagement/GroupManagementTables.kt:11`)
- Stores research groups/institutions
- Fields: groupName, institution, address fields, contactEmail

**UserGroupsTable** (`backend/src/main/kotlin/org/loculus/backend/service/groupmanagement/GroupManagementTables.kt:37`)
- Links users to groups
- Fields: userName, groupId (foreign key to GroupsTable)

## Data Use & Access Control

**DataUseTermsTable** (`backend/src/main/kotlin/org/loculus/backend/service/datauseterms/DataUseTermsTable.kt:15`)
- Tracks data usage terms per accession
- Fields: accession, changeDate, dataUseTermsType, restrictedUntil, userName

## File Management

**FilesTable** (`backend/src/main/kotlin/org/loculus/backend/service/files/FilesTable.kt:16`)
- Tracks files stored in S3
- Fields: id (UUID), uploadRequestedAt, uploader, groupId, releasedAt, size

## Citations & SeqSets

**SeqSetsTable** (`backend/src/main/kotlin/org/loculus/backend/service/seqsetcitations/SeqSetsTables.kt:6`)
- Manages sequence sets for citations
- Fields: seqSetId, seqSetVersion, name, description, seqSetDOI, createdAt, createdBy

**SeqSetRecordsTable** & **SeqSetToRecordsTable** (`backend/src/main/kotlin/org/loculus/backend/service/seqsetcitations/SeqSetsTables.kt:17,25`)
- Link sequence sets to individual records

## Service Layer Models

**SubmitModel** (`backend/src/main/kotlin/org/loculus/backend/model/SubmitModel.kt:85`)
- Service class for handling submissions
- Processes metadata/sequence file uploads
- Handles both original submissions and revisions

**ReleasedDataModel** (`backend/src/main/kotlin/org/loculus/backend/model/ReleasedDataModel.kt:58`)
- Service for retrieving and formatting released sequence data
- Computes additional metadata fields and version status

## Additional Tables

**AuditLogTable** (`backend/src/main/kotlin/org/loculus/backend/log/AuditLogTable.kt`)
- Tracks system audit events

**UpdateTrackerTable** (`backend/src/main/kotlin/org/loculus/backend/service/submission/UpdateTrackerTable.kt`)
- Tracks database update timestamps for ETags

**SequenceEntriesPreprocessedDataTable** (`backend/src/main/kotlin/org/loculus/backend/service/submission/SequenceEntriesPreprocessedDataTable.kt`)
- Stores preprocessed sequence data

**CurrentProcessingPipelineTable** (`backend/src/main/kotlin/org/loculus/backend/service/submission/CurrentProcessingPipelineTable.kt`)
- Tracks processing pipeline versions

**MetadataUploadAuxTable** & **SequenceUploadAuxTable** (`backend/src/main/kotlin/org/loculus/backend/service/submission/`)
- Temporary tables for batch upload processing

## Key Relationships

1. **Groups ↔ Users**: One-to-many relationship through UserGroupsTable
2. **Groups ↔ Sequences**: Sequences belong to groups via group_id foreign key
3. **Groups ↔ Files**: Files belong to groups for access control
4. **Sequences ↔ External Metadata**: One-to-many relationship for metadata updates
5. **Sequences ↔ Preprocessed Data**: One-to-one relationship for processed sequence data
6. **SeqSets ↔ Records**: Many-to-many relationship through SeqSetToRecordsTable

## Notes

- The models use Exposed's DSL for type-safe SQL operations
- JSONB columns are used for flexible metadata storage
- Primary keys are composite where versioning is involved (accession, version)
- Foreign key relationships maintain referential integrity between related tables
- The schema supports multi-version sequence entries with proper version tracking
- Temporary AUX tables are used during batch upload operations and cleaned up after processing