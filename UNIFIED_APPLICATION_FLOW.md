# Document Template Processor - Unified Application Flow

## 🔄 Complete End-to-End Application Flow

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                              DOCUMENT TEMPLATE PROCESSOR - UNIFIED FLOW                                                                      │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

    USER                    FRONTEND                    API ROUTER                DOCUMENT PROCESSOR           EXTERNAL SERVICES              STORAGE LAYER              DATABASE
     │                         │                           │                            │                           │                           │                         │
     │ 1. Select Template      │                           │                            │                           │                           │                         │
     ├────────────────────────►│                           │                            │                           │                           │                         │
     │                         │ 2. Validate File         │                            │                           │                           │                         │
     │                         ├─────────┐                 │                            │                           │                           │                         │
     │                         │         │                 │                            │                           │                           │                         │
     │                         │◄────────┘                 │                            │                           │                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │ 3. POST /api/templates    │                            │                           │                           │                         │
     │                         ├──────────────────────────►│                            │                           │                           │                         │
     │                         │                           │ 4. Route to Handler        │                           │                           │                         │
     │                         │                           ├───────────────────────────►│                           │                           │                         │
     │                         │                           │                            │ 5. Check File Type        │                           │                         │
     │                         │                           │                            ├─────────┐                 │                           │                         │
     │                         │                           │                            │         │                 │                           │                         │
     │                         │                           │                            │◄────────┘                 │                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 6a. Extract DOCX Placeholders                        │                         │
     │                         │                           │                            ├──────────────────────────►│                           │                         │
     │                         │                           │                            │                           │ 7a. Easy-Template-X       │                         │
     │                         │                           │                            │                           ├─────────┐                 │                         │
     │                         │                           │                            │                           │         │                 │                         │
     │                         │                           │                            │ 8a. Return Placeholders   │◄────────┘                 │                         │
     │                         │                           │                            │◄──────────────────────────┤                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 6b. Extract XLSX Placeholders                        │                         │
     │                         │                           │                            ├──────────────────────────►│                           │                         │
     │                         │                           │                            │                           │ 7b. ExcelJS Processing    │                         │
     │                         │                           │                            │                           ├─────────┐                 │                         │
     │                         │                           │                            │                           │         │                 │                         │
     │                         │                           │                            │ 8b. Return Placeholders   │◄────────┘                 │                         │
     │                         │                           │                            │◄──────────────────────────┤                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 9. Upload to Storage      │                           │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────►│                         │
     │                         │                           │                            │                           │                           │ 10. Supabase Upload      │
     │                         │                           │                            │                           │                           ├─────────┐               │
     │                         │                           │                            │                           │                           │         │               │
     │                         │                           │                            │ 11. Storage URL           │                           │◄────────┘               │
     │                         │                           │                            │◄──────────────────────────────────────────────────────┤                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 12. Save Template Record │                           │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────────────────────────────►│
     │                         │                           │                            │                           │                           │                         │ 13. Database Insert
     │                         │                           │                            │                           │                           │                         ├─────────┐
     │                         │                           │                            │                           │                           │                         │         │
     │                         │                           │                            │ 14. Template ID           │                           │                         │◄────────┘
     │                         │                           │                            │◄──────────────────────────────────────────────────────────────────────────────┤
     │                         │                           │ 15. Success Response       │                           │                           │                         │
     │                         │                           │◄───────────────────────────┤                           │                           │                         │
     │                         │ 16. Template + Placeholders│                           │                           │                           │                         │
     │                         │◄──────────────────────────┤                           │                           │                           │                         │
     │ 17. Show Placeholder Form│                          │                           │                           │                           │                         │
     │◄────────────────────────┤                           │                           │                           │                           │                         │
     │                         │                           │                           │                           │                           │                         │
     │ 18. Fill Form Data      │                           │                           │                           │                           │                         │
     ├────────────────────────►│                           │                           │                           │                           │                         │
     │                         │ 19. Validate Form         │                           │                           │                           │                         │
     │                         ├─────────┐                 │                           │                           │                           │                         │
     │                         │         │                 │                           │                           │                           │                         │
     │                         │◄────────┘                 │                           │                           │                           │                         │
     │                         │                           │                           │                           │                           │                         │
     │                         │ 20. POST /api/documents/generate                      │                           │                           │                         │
     │                         ├──────────────────────────►│                           │                           │                           │                         │
     │                         │                           │ 21. Route to Generation  │                           │                           │                         │
     │                         │                           ├───────────────────────────►│                           │                           │                         │
     │                         │                           │                            │ 22. Fetch Template       │                           │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────────────────────────────►│
     │                         │                           │                            │                           │                           │                         │ 23. Get Template Record
     │                         │                           │                            │                           │                           │                         ├─────────┐
     │                         │                           │                            │                           │                           │                         │         │
     │                         │                           │                            │ 24. Template Metadata    │                           │                         │◄────────┘
     │                         │                           │                            │◄──────────────────────────────────────────────────────────────────────────────┤
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 25. Download Template File│                          │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────►│                         │
     │                         │                           │                            │                           │                           │ 26. Retrieve File        │
     │                         │                           │                            │                           │                           ├─────────┐               │
     │                         │                           │                            │                           │                           │         │               │
     │                         │                           │                            │ 27. File Buffer          │                           │◄────────┘               │
     │                         │                           │                            │◄──────────────────────────────────────────────────────┤                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 28a. Process DOCX Document│                          │                         │
     │                         │                           │                            ├──────────────────────────►│                           │                         │
     │                         │                           │                            │                           │ 29a. Easy-Template-X     │                         │
     │                         │                           │                            │                           │     Replace Placeholders │                         │
     │                         │                           │                            │                           ├─────────┐                 │                         │
     │                         │                           │                            │                           │         │                 │                         │
     │                         │                           │                            │ 30a. Processed DOCX      │◄────────┘                 │                         │
     │                         │                           │                            │◄──────────────────────────┤                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 28b. Process XLSX Document│                          │                         │
     │                         │                           │                            ├──────────────────────────►│                           │                         │
     │                         │                           │                            │                           │ 29b. ExcelJS             │                         │
     │                         │                           │                            │                           │     Replace Placeholders │                         │
     │                         │                           │                            │                           ├─────────┐                 │                         │
     │                         │                           │                            │                           │         │                 │                         │
     │                         │                           │                            │ 30b. Processed XLSX      │◄────────┘                 │                         │
     │                         │                           │                            │◄──────────────────────────┤                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 31. Convert to PDF (Optional)                        │                         │
     │                         │                           │                            ├──────────────────────────►│                           │                         │
     │                         │                           │                            │                           │ 32a. Try LibreOffice     │                         │
     │                         │                           │                            │                           ├─────────┐                 │                         │
     │                         │                           │                            │                           │         │                 │                         │
     │                         │                           │                            │ 33a. PDF Success         │◄────────┘                 │                         │
     │                         │                           │                            │◄──────────────────────────┤                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │                           │ 32b. LibreOffice Failed  │                         │
     │                         │                           │                            │                           │      Try JavaScript       │                         │
     │                         │                           │                            │                           ├─────────┐                 │                         │
     │                         │                           │                            │                           │         │                 │                         │
     │                         │                           │                            │ 33b. PDF Fallback        │◄────────┘                 │                         │
     │                         │                           │                            │◄──────────────────────────┤                           │                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 34. Upload Generated Documents                       │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────►│                         │
     │                         │                           │                            │                           │                           │ 35a. Store Original      │
     │                         │                           │                            │                           │                           ├─────────┐               │
     │                         │                           │                            │                           │                           │         │               │
     │                         │                           │                            │                           │                           │◄────────┘               │
     │                         │                           │                            │                           │                           │ 35b. Store PDF           │
     │                         │                           │                            │                           │                           ├─────────┐               │
     │                         │                           │                            │                           │                           │         │               │
     │                         │                           │                            │ 36. Storage URLs          │                           │◄────────┘               │
     │                         │                           │                            │◄──────────────────────────────────────────────────────┤                         │
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 37. Create Document Record│                          │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────────────────────────────►│
     │                         │                           │                            │                           │                           │                         │ 38. Insert Document
     │                         │                           │                            │                           │                           │                         ├─────────┐
     │                         │                           │                            │                           │                           │                         │         │
     │                         │                           │                            │ 39. Document Created     │                           │                         │◄────────┘
     │                         │                           │                            │◄──────────────────────────────────────────────────────────────────────────────┤
     │                         │                           │ 40. Generation Complete   │                           │                           │                         │
     │                         │                           │◄───────────────────────────┤                           │                           │                         │
     │                         │ 41. Document + Download URLs                          │                           │                           │                         │
     │                         │◄──────────────────────────┤                           │                           │                           │                         │
     │ 42. Show Download Options│                          │                           │                           │                           │                         │
     │◄────────────────────────┤                           │                           │                           │                           │                         │
     │                         │                           │                           │                           │                           │                         │
     │ 43. Click Download      │                           │                           │                           │                           │                         │
     ├────────────────────────►│                           │                           │                           │                           │                         │
     │                         │ 44. GET /download/[id]    │                           │                           │                           │                         │
     │                         ├──────────────────────────►│                           │                           │                           │                         │
     │                         │                           │ 45. Route to Download     │                           │                           │                         │
     │                         │                           ├───────────────────────────►│                           │                           │                         │
     │                         │                           │                            │ 46. Fetch Document       │                           │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────────────────────────────►│
     │                         │                           │                            │                           │                           │                         │ 47. Get Document Record
     │                         │                           │                            │                           │                           │                         ├─────────┐
     │                         │                           │                            │                           │                           │                         │         │
     │                         │                           │                            │ 48. Document Metadata    │                           │                         │◄────────┘
     │                         │                           │                            │◄──────────────────────────────────────────────────────────────────────────────┤
     │                         │                           │                            │                           │                           │                         │
     │                         │                           │                            │ 49. Download File         │                           │                         │
     │                         │                           │                            ├──────────────────────────────────────────────────────►│                         │
     │                         │                           │                            │                           │                           │ 50. Stream File          │
     │                         │                           │                            │                           │                           ├─────────┐               │
     │                         │                           │                            │                           │                           │         │               │
     │                         │                           │                            │ 51. File Stream          │                           │◄────────┘               │
     │                         │                           │                            │◄──────────────────────────────────────────────────────┤                         │
     │                         │                           │ 52. File Response         │                           │                           │                         │
     │                         │                           │◄───────────────────────────┤                           │                           │                         │
     │                         │ 53. Download Stream       │                           │                           │                           │                         │
     │                         │◄──────────────────────────┤                           │                           │                           │                         │
     │ 54. File Downloaded     │                           │                           │                           │                           │                         │
     │◄────────────────────────┤                           │                           │                           │                           │                         │
     │                         │                           │                           │                           │                           │                         │

┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                 ERROR HANDLING FLOWS                                                                                         │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

    ERROR SCENARIOS              DETECTION POINT               FALLBACK ACTION                 RECOVERY MECHANISM              FINAL RESPONSE
         │                           │                            │                               │                               │
  File Validation Error     ─────────►│ Frontend/API Router      ├──────────────────────────────►│ Return Error Message          ├──────────────────────────────►│ User Retry
         │                           │                            │                               │                               │
  LibreOffice PDF Failed    ─────────►│ Document Processor       ├──────────────────────────────►│ JavaScript PDF Conversion    ├──────────────────────────────►│ PDF or Original
         │                           │                            │                               │                               │
  Storage Upload Failed     ─────────►│ Supabase Storage         ├──────────────────────────────►│ Retry Logic                  ├──────────────────────────────►│ Error or Success
         │                           │                            │                               │                               │
  Database Insert Failed    ─────────►│ Database Layer           ├──────────────────────────────►│ Transaction Rollback         ├──────────────────────────────►│ Consistency Error
         │                           │                            │                               │                               │
  Processing Engine Failed  ─────────►│ Document Processor       ├──────────────────────────────►│ Return Original Format       ├──────────────────────────────►│ Limited Functionality
         │                           │                            │                               │                               │

┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                               SERVICE RESPONSIBILITIES                                                                                        │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

FRONTEND              │  API ROUTER           │  DOCUMENT PROCESSOR      │  EXTERNAL SERVICES    │  STORAGE LAYER        │  DATABASE
                      │                       │                          │                       │                       │
• User Interface      │  • Route Handling     │  • File Type Detection   │  • Easy-Template-X    │  • Supabase Storage   │  • PostgreSQL
• Form Validation     │  • Request Validation │  • Placeholder Extraction│  • ExcelJS            │  • File Upload        │  • Template Records
• Progress Display    │  • Response Formatting│  • Document Processing   │  • LibreOffice        │  • File Download      │  • Document Records
• Error Handling      │  • Authentication     │  • PDF Conversion        │  • JavaScript PDF     │  • URL Generation     │  • Relationships
• State Management    │  • CORS Handling      │  • Storage Coordination  │  • System Commands    │  • Bucket Management  │  • Transactions
• Download Management │  • Error Propagation  │  • Database Coordination │  • Process Management │  • Security Policies  │  • Query Optimization
```

This unified flow diagram shows the complete journey from user interaction to file download, including all service handovers, error handling paths, and recovery mechanisms in a single comprehensive visualization.