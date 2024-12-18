I'll create a comprehensive markdown-formatted document explaining the function dependencies in the project.

# Function Dependencies in Educational Question Generation System

## Overview of Dependencies

### 1. Configuration Class (`Config`)

```markdown
## `__init__()` Method
- Dependencies:
  * Calls `_detect_levels()` method
  * Requires environment variables
  * Initializes:
    - Embedding model
    - Language model

## `_detect_levels()` Method
- Independent method
- Relies on:
  * `os.path` module
  * No direct function dependencies
```

### 2. Vector Store Class (`VectorStore`)

```markdown
## `__init__()` Method
- Dependencies:
  * Requires embedding model
  * Initializes:
    - `vector_store`
    - `available_levels`

## `_scan_data_levels()` Method
- Independent method
- Uses:
  * `os` module for directory scanning
  * No internal function dependencies

## `_create_vector_store_for_level()` Method
- Complex dependencies:
  * Requires:
    - PyPDFLoader
    - RecursiveCharacterTextSplitter
    - FAISS vector store
  * Indirect dependencies:
    - Successful PDF file scanning
    - Embedding model for vector creation

## `update_available_levels()` Method
- Depends on multiple methods:
  1. `_scan_data_levels()`
     - Gets initial list of levels
  2. `_create_vector_store_for_level()`
     - Creates vector stores for each level

## `load_vector_store()` Method
- Depends on:
  1. `update_available_levels()`
     - Refreshes available levels
  2. `FAISS.load_local()`
     - Loads existing vector store
```

### 3. Question Generator Class (`QuestionGenerator`)

```markdown
## `__init__()` Method
- Dependencies:
  * Requires:
    - Language model (llm)
    - Prompt templates

## `generate_question()` Method
- Orchestrator method
- Depends on specific generation methods:
  1. `_generate_mcq()` for MCQ questions
  2. `_generate_true_false()` for True/False questions
  3. `_generate_essay()` for essay questions

## Specific Generation Methods
### `_generate_mcq()`
- Dependencies:
  * MCQ question prompt template
  * Language model
  * Text parsing logic

### `_generate_true_false()`
- Dependencies:
  * True/False prompt template
  * Language model
  * Text parsing logic

### `_generate_essay()`
- Dependencies:
  * Essay prompt template
  * Language model
  * Text parsing logic
```

### 4. Answer Checker Class (`AnswerChecker`)

```markdown
## `__init__()` Method
- Dependencies:
  * Requires:
    - Language model
    - Prompt templates

## `check_answer()` Method
- Comprehensive dependencies:
  1. Answer checking prompt template
  2. Language model
  3. Generates constructive feedback
```

## Dependency Hierarchy Diagram

```
Config
└── _detect_levels()
    └── Scans data directory

VectorStore
├── _scan_data_levels()
│   └── Lists directory contents
├── _create_vector_store_for_level()
│   ├── Uses PyPDFLoader
│   ├── Uses RecursiveCharacterTextSplitter
│   └── Uses FAISS for vector creation
├── update_available_levels()
│   ├── Calls _scan_data_levels()
│   └── Calls _create_vector_store_for_level()
└── load_vector_store()
    └── Calls update_available_levels()

QuestionGenerator
├── generate_question()
│   ├── Calls _generate_mcq()
│   ├── Calls _generate_true_false()
│   └── Calls _generate_essay()
├── _generate_mcq()
│   └── Uses language model
├── _generate_true_false()
│   └── Uses language model
└── _generate_essay()
    └── Uses language model

AnswerChecker
└── check_answer()
    └── Uses language model for feedback
```

## Key Interdependency Insights

### 1. Configuration Flow
- `Config` provides core system configuration
- Initializes models and detects available levels
- Other classes depend on this configuration

### 2. Vector Store Processing
1. Scan available levels
2. Create vector stores
3. Load specific level's vector store

### 3. Question Generation
- Requires context from `VectorStore`
- Uses prompt templates
- Leverages language model for intelligent generation

### 4. Answer Checking
- Depends on generated questions
- Provides nuanced, constructive feedback

## Execution Sequence
1. Initialize `Config`
2. Create `VectorStore`
3. Update/Load Vector Stores
4. Use `QuestionGenerator`
5. Use `AnswerChecker`

## Potential Improvements
- Implement robust error handling
- Add dependency injection
- Create abstract interfaces
- Enhance modularity

## Technical Recommendations
- Use type hints
- Implement comprehensive logging
- Add detailed docstrings
- Create unit tests for each method

  I'll provide a detailed workflow for the functions in the educational question generation system, using a clear, step-by-step markdown format.

# Workflow for Educational Question Generation System

## 1. System Initialization Workflow

```markdown
### Initialization Flow
1. Config Class Initialization
   - Load Google API Key
     ↓
   - Configure Generative AI
     ↓
   - Initialize Embedding Model
     ↓
   - Initialize Language Model
     ↓
   - Detect Available Educational Levels
```

## 2. Vector Store Preparation Workflow

```markdown
### Vector Store Preparation
1. Scan Data Levels
   ↓
2. Identify PDF Files in Each Level
   ↓
3. Process Document Workflow:
   - Load PDF Documents
   - Split Documents into Chunks
     ↓
   - Create Vector Embeddings
     ↓
   - Save Vector Stores Locally
```

## 3. Question Generation Workflow

```markdown
### Question Generation Process
1. Select Educational Level
   ↓
2. Load Vector Store for Selected Level
   ↓
3. Retrieve Contextual Document Chunks
   ↓
4. Question Type Selection
   - Multiple Choice (MCQ)
   - True/False
   - Essay
     ↓
5. Apply Appropriate Prompt Template
   ↓
6. Generate Question Using AI Model
   ↓
7. Parse and Structure Question
   - Extract Question Text
   - Create Answer Options
   - Identify Correct Answer
```

## 4. Answer Evaluation Workflow

```markdown
### Answer Checking Process
1. Receive User's Answer
   ↓
2. Retrieve Reference Answer
   ↓
3. Load Original Context
   ↓
4. Apply Answer Checking Prompt
   ↓
5. Generate Feedback Using AI Model
   ↓
6. Provide Constructive Feedback
```

## Comprehensive System Workflow

```markdown
### End-to-End Workflow
1. System Configuration
   ↓
2. Vector Store Preparation
   │
   ├── Scan Levels
   ├── Process Documents
   └── Create Embeddings
   ↓
3. Question Generation
   │
   ├── Select Level
   ├── Load Vector Store
   ├── Generate Question
   │   ├── MCQ
   │   ├── True/False
   │   └── Essay
   └── Prepare Answer Options
   ↓
4. User Interaction
   │
   ├── Present Question
   ├── Receive User Answer
   └── Evaluate Answer
       │
       ├── Compare with Reference
       └── Generate Feedback
```

## Detailed Function Interactions

### Config Class
```markdown
Workflow:
1. __init__()
   - Validate API Key
   - Configure Environment
   - Initialize Models
2. _detect_levels()
   - Scan Data Directory
   - Return Available Levels
```

### VectorStore Class
```markdown
Workflow:
1. _scan_data_levels()
   - List Directories
   - Identify Levels
2. _create_vector_store_for_level()
   - Load PDFs
   - Split Documents
   - Create Embeddings
3. update_available_levels()
   - Refresh Level List
   - Create Missing Vector Stores
4. load_vector_store()
   - Load Specific Level's Store
   - Prepare for Question Generation
```

### QuestionGenerator Class
```markdown
Workflow:
1. generate_question()
   - Route to Specific Generator
2. _generate_mcq()
   - Create Multiple Choice Question
3. _generate_true_false()
   - Create True/False Question
4. _generate_essay()
   - Create Essay Question
```

### AnswerChecker Class
```markdown
Workflow:
1. check_answer()
   - Receive User Answer
   - Compare with Reference
   - Generate Personalized Feedback
```

## Error Handling and Validation Workflow

```markdown
### Error Handling Steps
1. API Key Validation
   ↓
2. Data Directory Checks
   ↓
3. PDF Processing Validation
   ↓
4. Vector Store Creation
   ↓
5. Question Generation Validation
   ↓
6. Answer Evaluation Checks
```

## Performance Optimization Workflow

```markdown
### Optimization Strategies
1. Lazy Loading of Vector Stores
2. Caching Generated Questions
3. Efficient Embedding Computation
4. Modular Design for Scalability
```

## Recommended Workflow Enhancements

1. Add comprehensive logging
2. Implement robust error handling
3. Create modular, reusable components
4. Support multiple document formats
5. Enhance AI model integration

This workflow provides a comprehensive overview of how different functions interact and process educational content from initialization to answer evaluation.

Would you like me to elaborate on any specific part of the workflow?

---

