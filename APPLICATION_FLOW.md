# Document Template Processor - Application Flow Diagram

## 🔄 Complete Application Flow

### Phase 1: Template Upload Flow
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   User Action   │    │   Frontend      │    │   Backend       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │ 1. Select File        │                       │
         ├──────────────────────►│                       │
         │                       │ 2. File Validation    │
         │                       ├──────────────────────►│
         │                       │                       │ 3. Validate Type/Size
         │                       │                       ├─────────┐
         │                       │                       │         │
         │                       │                       │◄────────┘
         │                       │ 4. Upload Progress    │
         │                       │◄──────────────────────┤
         │ 5. Show Progress      │                       │
         │◄──────────────────────┤                       │
         │                       │                       │
```

### Phase 2: Placeholder Extraction Service Handover
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   API Router    │    │Document Processor│    │  Easy-Template-X │    │    ExcelJS      │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │                       │
         │ 1. POST /templates    │                       │                       │
         ├──────────────────────►│                       │                       │
         │                       │ 2. Check File Type    │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │                       │◄────────┘             │                       │
         │                       │                       │                       │
         │                       │ 3a. If DOCX           │                       │
         │                       ├──────────────────────►│                       │
         │                       │                       │ 4a. Extract Text      │
         │                       │                       ├─────────┐             │
         │                       │                       │         │             │
         │                       │                       │◄────────┘             │
         │                       │                       │ 5a. Find Placeholders │
         │                       │                       ├─────────┐             │
         │                       │                       │         │             │
         │                       │ 6a. Return Patterns   │◄────────┘             │
         │                       │◄──────────────────────┤                       │
         │                       │                       │                       │
         │                       │ 3b. If XLSX           │                       │
         │                       ├──────────────────────────────────────────────►│
         │                       │                       │                       │ 4b. Load Workbook
         │                       │                       │                       ├─────────┐
         │                       │                       │                       │         │
         │                       │                       │                       │◄────────┘
         │                       │                       │                       │ 5b. Scan Cells
         │                       │                       │                       ├─────────┐
         │                       │                       │                       │         │
         │                       │ 6b. Return Patterns   │                       │◄────────┘
         │                       │◄──────────────────────────────────────────────┤
         │ 7. Placeholders Found │                       │                       │
         │◄──────────────────────┤                       │                       │
```

### Phase 3: Storage Service Handover
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│Document Processor│    │Supabase Storage │    │   Database      │    │   Frontend      │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │                       │
         │ 1. Store Template     │                       │                       │
         ├──────────────────────►│                       │                       │
         │                       │ 2. Generate Filename  │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │                       │◄────────┘             │                       │
         │                       │ 3. Upload to Bucket   │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │ 4. Storage URL        │◄────────┘             │                       │
         │◄──────────────────────┤                       │                       │
         │                       │                       │                       │
         │ 5. Save Template Record                        │                       │
         ├──────────────────────────────────────────────►│                       │
         │                       │                       │ 6. Insert Template    │
         │                       │                       ├─────────┐             │
         │                       │                       │         │             │
         │                       │                       │◄────────┘             │
         │ 7. Template Created   │                       │                       │
         │◄──────────────────────────────────────────────┤                       │
         │                       │                       │                       │
         │ 8. Success Response   │                       │                       │
         ├──────────────────────────────────────────────────────────────────────►│
         │                       │                       │                       │ 9. Show Form
         │                       │                       │                       ├─────────┐
         │                       │                       │                       │         │
         │                       │                       │                       │◄────────┘
```

### Phase 4: Document Generation Flow
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   User Input    │    │   Frontend      │    │   Backend       │    │Document Processor│
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │                       │
         │ 1. Fill Placeholders  │                       │                       │
         ├──────────────────────►│                       │                       │
         │                       │ 2. Validate Form      │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │                       │◄────────┘             │                       │
         │                       │ 3. Submit Generation  │                       │
         │                       ├──────────────────────►│                       │
         │                       │                       │ 4. Start Processing   │
         │                       │                       ├──────────────────────►│
         │                       │                       │                       │ 5. Fetch Template
         │                       │                       │                       ├─────────┐
         │                       │                       │                       │         │
         │                       │                       │                       │◄────────┘
```

### Phase 5: Template Processing Service Handover
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│Document Processor│    │   Storage       │    │Processing Engine│    │   File System   │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │                       │
         │ 1. Download Template  │                       │                       │
         ├──────────────────────►│                       │                       │
         │                       │ 2. Retrieve File      │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │ 3. Template Buffer    │◄────────┘             │                       │
         │◄──────────────────────┤                       │                       │
         │                       │                       │                       │
         │ 4. Process Document   │                       │                       │
         ├──────────────────────────────────────────────►│                       │
         │                       │                       │ 5. Replace Placeholders│
         │                       │                       ├─────────┐             │
         │                       │                       │         │             │
         │                       │                       │◄────────┘             │
         │ 6. Processed Buffer   │                       │                       │
         │◄──────────────────────────────────────────────┤                       │
         │                       │                       │                       │
         │ 7. Write Temp File    │                       │                       │
         ├──────────────────────────────────────────────────────────────────────►│
         │                       │                       │                       │ 8. Create File
         │                       │                       │                       ├─────────┐
         │                       │                       │                       │         │
         │ 9. File Path          │                       │                       │◄────────┘
         │◄──────────────────────────────────────────────────────────────────────┤
```

### Phase 6: PDF Conversion Service Handover
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│Document Processor│    │  LibreOffice    │    │JavaScript Engine│    │   File System   │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │                       │
         │ 1. Convert to PDF     │                       │                       │
         ├──────────────────────►│                       │                       │
         │                       │ 2. Check Availability │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │                       │◄────────┘             │                       │
         │                       │ 3a. Execute Conversion │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │ 4a. PDF Success       │◄────────┘             │                       │
         │◄──────────────────────┤                       │                       │
         │                       │                       │                       │
         │ 3b. If LibreOffice Fails                      │                       │
         ├──────────────────────────────────────────────►│                       │
         │                       │                       │ 4b. JS Conversion     │
         │                       │                       ├─────────┐             │
         │                       │                       │         │             │
         │ 5b. PDF Fallback      │                       │◄────────┘             │
         │◄──────────────────────────────────────────────┤                       │
         │                       │                       │                       │
         │ 6. Store PDF          │                       │                       │
         ├──────────────────────────────────────────────────────────────────────►│
         │                       │                       │                       │ 7. Write PDF
         │                       │                       │                       ├─────────┐
         │                       │                       │                       │         │
         │ 8. PDF Path           │                       │                       │◄────────┘
         │◄──────────────────────────────────────────────────────────────────────┤
```

### Phase 7: Final Storage and Response Flow
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│Document Processor│    │Supabase Storage │    │   Database      │    │   Frontend      │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │                       │
         │ 1. Upload Documents   │                       │                       │
         ├──────────────────────►│                       │                       │
         │                       │ 2. Store Original     │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │                       │◄────────┘             │                       │
         │                       │ 3. Store PDF          │                       │
         │                       ├─────────┐             │                       │
         │                       │         │             │                       │
         │ 4. Storage URLs       │◄────────┘             │                       │
         │◄──────────────────────┤                       │                       │
         │                       │                       │                       │
         │ 5. Create Document Record                      │                       │
         ├──────────────────────────────────────────────►│                       │
         │                       │                       │ 6. Insert Document    │
         │                       │                       ├─────────┐             │
         │                       │                       │         │             │
         │                       │                       │◄────────┘             │
         │ 7. Document Created   │                       │                       │
         │◄──────────────────────────────────────────────┤                       │
         │                       │                       │                       │
         │ 8. Generation Complete                        │                       │
         ├──────────────────────────────────────────────────────────────────────►│
         │                       │                       │                       │ 9. Show Downloads
         │                       │                       │                       ├─────────┐
         │                       │                       │                       │         │
         │                       │                       │                       │◄────────┘
```

## 🔄 Service Control Handover Points

### 1. Frontend to Backend Handover
- **Trigger:** User action (upload/generate)
- **Control Transfer:** HTTP request with validation
- **Responsibility:** Frontend validates UI, Backend validates business logic

### 2. API Router to Document Processor Handover
- **Trigger:** Valid API request received
- **Control Transfer:** Service method call
- **Responsibility:** Router handles HTTP, Processor handles file operations

### 3. Document Processor to External Libraries Handover
- **Trigger:** File type detection
- **Control Transfer:** Library-specific method calls
- **Responsibility:** Processor orchestrates, Libraries handle format-specific operations

### 4. Document Processor to Storage Service Handover
- **Trigger:** Successful file processing
- **Control Transfer:** Storage API calls
- **Responsibility:** Processor handles logic, Storage handles persistence

### 5. Document Processor to Database Handover
- **Trigger:** Successful storage operations
- **Control Transfer:** ORM method calls
- **Responsibility:** Processor provides data, Database ensures consistency

### 6. PDF Conversion Service Chain
- **Primary:** LibreOffice system command
- **Fallback:** JavaScript PDF libraries
- **Error Handling:** Graceful degradation between services

## 🚨 Error Handling and Recovery Points

### Service Failure Recovery Matrix
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  Primary Path   │    │  Fallback Path  │    │  Error Response │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
LibreOffice PDF ────────────────► JavaScript PDF ──────► Original Format Only
         │                       │                       │
Supabase Storage ───────────────► Local Storage ────────► Upload Failed
         │                       │                       │
Template Processing ────────────► Manual Processing ────► Processing Failed
         │                       │                       │
Database Insert ────────────────► Retry Logic ──────────► Data Consistency Error
```

This application flow diagram shows the complete journey from user interaction through all service handovers, including error handling and fallback mechanisms. Each phase demonstrates how control is transferred between different components and services in the document processing pipeline.