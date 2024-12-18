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

---

