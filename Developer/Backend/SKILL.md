---
name: backend-impl
description: Implement backend components from DESIGN.md with well-formatted commits and rustdoc documentation
metadata:
  style: Precise
  audience: backend developers
  primary_language: Rust
  secondary_languages: Go, Python, C++
---

# Backend Implementation Skill

Implements backend components from technical design documents (**DESIGN.md**)
with well-structured git commits and comprehensive documentation following Rust
conventions and rustdoc style.

## Usage
```
/backend-impl <component-name>
```

Example:
```
/backend-impl llm-client
/backend-impl session-store
```

## Workflow

### 1. Component Selection & Analysis
- READ **README.md** to understand the functionalities of the current repo. This
is possibily gitignored. So use `rg --files --no-ignore` 
- Read **DESIGN.md** to identify all frontend components. This is possibily
gitignored. So use `rg --files --no-ignore`
- Select the requested component from the architecture. If the component is not
  found, ask the user with close alternatives and only proceed after user
  confirmation
- Extract requirements, interfaces, and dependencies
- Identify the target language (prefer Rust, fallback to Go or Python or C++ if specified)

### 2. Implementation Planning
- Break down component into logical implementation units
- Plan commit sequence following dependency order:
  1. Core types and traits/interfaces
  2. Error types and result handling
  3. Data structures and models
  4. Core business logic implementation
  5. API handlers or service interfaces
  6. Integration adapters
  7. Tests (optional for PoC)
- Separate code commits from documentation commits

### 3. Code Implementation
For each implementation unit:
- Write idiomatic Rust code following The Rust Book conventions
- Use type system for correctness (Result, Option, strong typing)
- Follow Rust naming conventions (snake_case for functions/variables, PascalCase for types)
- Leverage Rust's ownership and borrowing for safety
- Include succinct error handling using Result types
- Avoid over-engineering on error handling. Focus on core functionality logic
- Add inline comments for complex logic only
- Create conventional commit following the `/commit` skill

### 4. Documentation Generation
After code implementation:
- Generate well-formatted rustdoc documentation in the code
- Document all public modules, structs, enums, traits, and functions
- Include parameter descriptions, return values, error cases, and examples
- Create separate documentation commit(s)

## Commit Strategy

### Code Commits (Conventional Format)
```
<type>(<scope>): <description>

[detailed changes]

[references]
```

**Types for backend work:**
- `feat`: New component, module, or feature
- `fix`: Bug fixes
- `refactor`: Code restructuring
- `perf`: Performance improvements
- `test`: Unit or integration tests
- `chore`: Dependencies, tooling, build config

**Scopes:**
- Component name (e.g., `llm-client`, `session-store`)
- Module area (e.g., `api`, `db`, `error`, `service`)

### Documentation Commits (Separate)
```
docs(<component>): add rustdoc documentation

- Document public API surface
- Add usage examples
- Include error handling patterns
```

**Rules:**
- Documentation commits MUST be separate from code commits
- Generate docs AFTER implementation is complete
- One docs commit per component (or logical grouping)

## Documentation Format (Rustdoc Style)

### Module Documentation
```rust
//! Module Name - Brief description
//!
//! Longer description of the module's purpose,
//! responsibilities, and design decisions.
//!
//! # Examples
//!
//! ```
//! use crate::module_name::{TypeName, function_name};
//!
//! let instance = TypeName::new("example");
//! let result = function_name(&instance)?;
//! ```
//!
//! # Architecture
//!
//! This module provides [specific functionality] and integrates
//! with [other components]. See DESIGN.md for overall architecture.
```

### Struct/Enum Documentation
```rust
/// Brief description of the struct
///
/// Longer description explaining the purpose, usage patterns,
/// and any important invariants or constraints.
///
/// # Fields
///
/// * `field1` - Description of field1
/// * `field2` - Description of field2
///
/// # Examples
///
/// ```
/// let instance = StructName {
///     field1: "value",
///     field2: 42,
/// };
/// ```
#[derive(Debug, Clone)]
pub struct StructName {
    /// Field1 description
    pub field1: String,
    /// Field2 description
    field2: i32,
}
```

### Trait Documentation
```rust
/// Brief description of the trait
///
/// This trait defines the interface for [specific behavior].
/// Implementors must provide [required functionality].
///
/// # Examples
///
/// ```
/// use crate::TraitName;
///
/// struct MyImpl;
///
/// impl TraitName for MyImpl {
///     fn method(&self) -> Result<(), Error> {
///         Ok(())
///     }
/// }
/// ```
pub trait TraitName {
    /// Brief description of method
    ///
    /// # Arguments
    ///
    /// * `param` - Description of parameter
    ///
    /// # Returns
    ///
    /// Description of return value
    ///
    /// # Errors
    ///
    /// Returns `Err` if [condition]
    fn method(&self, param: &str) -> Result<String, Error>;
}
```

### Function Documentation
```rust
/// Brief description of function
///
/// Longer description of what the function does,
/// its behavior, and any side effects.
///
/// # Arguments
///
/// * `param1` - Description of param1
/// * `param2` - Description of param2
///
/// # Returns
///
/// Description of return value and its meaning
///
/// # Errors
///
/// * `ErrorType::Variant1` - When [condition]
/// * `ErrorType::Variant2` - When [condition]
///
/// # Examples
///
/// ```
/// use crate::function_name;
///
/// let result = function_name("input", 42)?;
/// assert_eq!(result, expected_value);
/// ```
///
/// # Panics
///
/// This function panics if [condition] (only if applicable)
pub fn function_name(param1: &str, param2: i32) -> Result<String, Error> {
    // implementation
}
```

### Error Documentation
```rust
/// Errors that can occur in this module
///
/// This enum represents all possible error conditions
/// when working with [component name].
#[derive(Debug, thiserror::Error)]
pub enum Error {
    /// Network request failed
    ///
    /// Returned when the HTTP request encounters a network error
    #[error("network request failed: {0}")]
    NetworkError(#[from] reqwest::Error),
    
    /// Invalid configuration
    ///
    /// Returned when required configuration is missing or malformed
    #[error("invalid config: {0}")]
    ConfigError(String),
    
    /// Resource not found
    #[error("resource not found: {0}")]
    NotFound(String),
}
```

### Documentation Sections
Each component documentation needs to include:

1. **Module Overview** (at top of file with `//!`)
   - Purpose and responsibility
   - Position in architecture
   - Key design decisions

2. **Public API** (for public items)
   - All public structs, enums, traits
   - Public functions and methods
   - Type parameters and constraints

3. **Error Handling**
   - Error types defined
   - Error propagation patterns
   - Recovery strategies

4. **Integration Points**
   - Dependencies on other modules
   - Trait implementations expected
   - Configuration requirements

5. **Usage Examples**
   - Basic usage with `# Examples` section
   - Common patterns
   - Error handling examples

## Rust-Specific Guidelines

### Code Style
- Follow [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- Use `rustfmt` for consistent formatting
- Run `clippy` and fix warnings
- Prefer `&str` over `String` for parameters
- Use `impl Trait` for return types when appropriate
- Leverage the type system: avoid primitive obsession

### Error Handling
- Define custom error types using `thiserror`
- Use `Result<T, E>` for fallible operations
- Use `Option<T>` for nullable values
- Propagate errors with `?` operator
- Avoid `unwrap()` and `expect()` in library code

### Async Code
- Use `tokio` as default async runtime
- Mark async functions with proper docs about runtime requirements
- Document cancellation safety
- Use `async-trait` for async trait methods if needed

### Memory & Performance
- Use references and borrowing by default
- Clone only when necessary and document why
- Use `Cow` for flexible owned/borrowed data
- Leverage zero-cost abstractions
- Don't over-optimize for PoC, but avoid obvious inefficiencies

### Testing
- Write doc tests in `# Examples` sections (they run with `cargo test`)
- Add unit tests in same file with `#[cfg(test)] mod tests`
- Integration tests in `tests/` directory (optional for PoC)

## Example Output

### Commit Sequence for "LLM Client Component"
```
1. feat(llm-client): add core types and traits
2. feat(llm-client): implement error types
3. feat(llm-client): add request/response models
4. feat(llm-client): implement mock client
5. feat(llm-client): implement remote HTTP client
6. feat(llm-client): add timeout and retry logic
7. docs(llm-client): add rustdoc documentation
```

### Sample Documentation Output
```rust
// File: src/llm_client.rs

//! LLM Client - Interface for interacting with language model services
//!
//! This module provides a unified interface for making requests to
//! language model services, with both mock and production implementations.
//! Supports timeout handling, request/response mapping, and error recovery.
//!
//! # Examples
//!
//! ```
//! use crate::llm_client::{LlmClient, MockLlmClient, LlmRequest};
//!
//! # async fn example() -> Result<(), Box<dyn std::error::Error>> {
//! let client = MockLlmClient::new();
//! let request = LlmRequest {
//!     document_id: "doc123".into(),
//!     selection_text: "selected text".into(),
//!     user_prompt: "Explain this".into(),
//!     context: None,
//! };
//!
//! let response = client.send(request).await?;
//! println!("Reply: {}", response.reply_text);
//! # Ok(())
//! # }
//! ```
//!
//! # Architecture
//!
//! The client follows a trait-based design allowing different implementations.
//! See DESIGN.md section "LLM Service" for integration details.

use async_trait::async_trait;
use serde::{Deserialize, Serialize};
use thiserror::Error;

/// Errors that can occur when interacting with LLM services
#[derive(Debug, Error)]
pub enum LlmError {
    /// Network or HTTP request failed
    #[error("network error: {0}")]
    NetworkError(#[from] reqwest::Error),
    
    /// Authentication failed (invalid or missing API key)
    #[error("authentication failed: {0}")]
    AuthError(String),
    
    /// Request timed out
    #[error("request timed out after {0}s")]
    Timeout(u64),
    
    /// Invalid response from service
    #[error("invalid response: {0}")]
    InvalidResponse(String),
}

/// Request to an LLM service
///
/// Contains the context and prompt for generating a response.
///
/// # Fields
///
/// * `document_id` - Unique identifier for the document
/// * `selection_text` - Text selected by the user
/// * `user_prompt` - User's question or instruction
/// * `context` - Optional additional context
#[derive(Debug, Clone, Serialize, Deserialize)]
pub struct LlmRequest {
    pub document_id: String,
    pub selection_text: String,
    pub user_prompt: String,
    #[serde(skip_serializing_if = "Option::is_none")]
    pub context: Option<String>,
}

/// Response from an LLM service
///
/// # Fields
///
/// * `reply_text` - Generated text response
#[derive(Debug, Clone, Serialize, Deserialize)]
pub struct LlmResponse {
    pub reply_text: String,
}

/// Client interface for LLM services
///
/// This trait defines the contract for sending requests to language models.
/// Implementations handle the specific details of mock vs. production clients.
///
/// # Examples
///
/// ```
/// use crate::llm_client::{LlmClient, LlmRequest};
///
/// async fn use_client<C: LlmClient>(client: &C) -> Result<String, C::Error> {
///     let request = LlmRequest {
///         document_id: "doc1".into(),
///         selection_text: "text".into(),
///         user_prompt: "summarize".into(),
///         context: None,
///     };
///     
///     let response = client.send(request).await?;
///     Ok(response.reply_text)
/// }
/// ```
#[async_trait]
pub trait LlmClient: Send + Sync {
    /// Error type for this client
    type Error;

    /// Send a request to the LLM service
    ///
    /// # Arguments
    ///
    /// * `request` - The LLM request containing prompt and context
    ///
    /// # Returns
    ///
    /// The generated response from the model
    ///
    /// # Errors
    ///
    /// * Returns `Err` if the network request fails
    /// * Returns `Err` if authentication fails
    /// * Returns `Err` if the request times out
    async fn send(&self, request: LlmRequest) -> Result<LlmResponse, Self::Error>;
}

/// Mock LLM client for testing and development
///
/// Returns canned responses without making network calls.
/// Useful for local development and unit tests.
///
/// # Examples
///
/// ```
/// use crate::llm_client::{MockLlmClient, LlmClient, LlmRequest};
///
/// # async fn example() -> Result<(), LlmError> {
/// let client = MockLlmClient::new();
/// let response = client.send(LlmRequest {
///     document_id: "test".into(),
///     selection_text: "text".into(),
///     user_prompt: "prompt".into(),
///     context: None,
/// }).await?;
/// # Ok(())
/// # }
/// ```
pub struct MockLlmClient;

impl MockLlmClient {
    /// Create a new mock client
    pub fn new() -> Self {
        Self
    }
}

#[async_trait]
impl LlmClient for MockLlmClient {
    type Error = LlmError;

    async fn send(&self, request: LlmRequest) -> Result<LlmResponse, Self::Error> {
        // Simulate processing delay
        tokio::time::sleep(tokio::time::Duration::from_millis(100)).await;
        
        Ok(LlmResponse {
            reply_text: format!(
                "Mock response to: {} | Selection: {}",
                request.user_prompt, request.selection_text
            ),
        })
    }
}

/// Production LLM client using HTTP API
///
/// Makes authenticated requests to a remote LLM endpoint.
/// Supports timeout configuration and retry logic.
///
/// # Examples
///
/// ```no_run
/// use crate::llm_client::{RemoteLlmClient, LlmClient};
///
/// # async fn example() -> Result<(), Box<dyn std::error::Error>> {
/// let client = RemoteLlmClient::new(
///     "https://api.example.com/v1/generate",
///     "sk-your-api-key",
///     30, // timeout in seconds
/// );
/// # Ok(())
/// # }
/// ```
pub struct RemoteLlmClient {
    endpoint: String,
    api_key: String,
    timeout_secs: u64,
    client: reqwest::Client,
}

impl RemoteLlmClient {
    /// Create a new remote LLM client
    ///
    /// # Arguments
    ///
    /// * `endpoint` - Base URL of the LLM API
    /// * `api_key` - Authentication API key
    /// * `timeout_secs` - Request timeout in seconds
    ///
    /// # Examples
    ///
    /// ```
    /// use crate::llm_client::RemoteLlmClient;
    ///
    /// let client = RemoteLlmClient::new(
    ///     "https://api.llm.com/v1",
    ///     "key123",
    ///     60
    /// );
    /// ```
    pub fn new(endpoint: impl Into<String>, api_key: impl Into<String>, timeout_secs: u64) -> Self {
        Self {
            endpoint: endpoint.into(),
            api_key: api_key.into(),
            timeout_secs,
            client: reqwest::Client::new(),
        }
    }
}

#[async_trait]
impl LlmClient for RemoteLlmClient {
    type Error = LlmError;

    async fn send(&self, request: LlmRequest) -> Result<LlmResponse, Self::Error> {
        let response = self.client
            .post(&self.endpoint)
            .header("Authorization", format!("Bearer {}", self.api_key))
            .json(&request)
            .timeout(std::time::Duration::from_secs(self.timeout_secs))
            .send()
            .await
            .map_err(|e| {
                if e.is_timeout() {
                    LlmError::Timeout(self.timeout_secs)
                } else {
                    LlmError::NetworkError(e)
                }
            })?;

        if response.status() == 401 {
            return Err(LlmError::AuthError("invalid API key".into()));
        }

        response.json::<LlmResponse>()
            .await
            .map_err(|e| LlmError::InvalidResponse(e.to_string()))
    }
}
```

## Quality Standards

### Code Quality
- Follow [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- Use `rustfmt` for consistent formatting (run before commits)
- Run `clippy` and address warnings
- Maintain consistent naming following Rust conventions
- Write self-documenting code with clear type and variable names
- Add comments for non-obvious logic only
- Handle errors appropriately; don't over-engineer for PoC

### Documentation Quality
- Use clear, concise language. Try to be as succinct as possible
- Provide realistic, runnable examples in doc comments
- Document all public APIs with rustdoc comments (`///` or `//!`)
- Explain complex algorithms or patterns succinctly
- Keep documentation in sync with code but in **DIFFERENT** commits

### Commit Quality
- Atomic commits (one logical change per commit)
- Clear, descriptive commit messages
- Reference design document sections where applicable
- Maintain linear, logical commit history
- No "WIP" or "fix" commits in final sequence

## Error Handling

When implementation encounters issues:
- Document assumptions made when specs are ambiguous
- Note missing dependencies or prerequisites
- Suggest improvements to design document
- Flag potential performance or security concerns, but for PoC don't over-engineer

## Deliverables

For each component implementation:
1. Working Rust code committed in logical sequence
2. All public APIs documented with rustdoc comments
3. Separate documentation commit(s)
4. Doc tests in examples where applicable
5. Examples provided in rustdoc
6. Component integration verified
7. Summary of implementation decisions and tradeoffs in the code commit

## Integration with Other Components

- Read the doc generated from frontend if possible before implementing
- Ensure API contracts match frontend expectations (types, error handling)

## Integration with Other Skills

- Uses `/commit` skill for all git commits
- Can invoke testing skills for component validation

## Notes

- Always read **README.md** and **DESIGN.md** before starting implementation.
This is possibily gitignored. So use `rg --files --no-ignore`
- **Prefer Rust** as the default implementation language unless specified
  otherwise
- Keep code commits and doc commits strictly separated
- Follow existing project structure and Cargo workspace conventions
- Run `cargo fmt` and `cargo clippy` before committing
- Update component status (what is implemented and what is WIP) in **IMPL.md**
  when complete. Note that **IMPL.md** is possibly gitignored. So use `rg
  --files --no-ignore`
- Use standard Rust project structure:
  - `src/lib.rs` or `src/main.rs` as entry point
  - `src/<component>.rs` for modules
  - `tests/` for integration tests
  - `Cargo.toml` for dependencies
- Common dependencies for backend:
  - `tokio` - async runtime
  - `serde`, `serde_json` - serialization
  - `thiserror` - error handling
  - `reqwest` - HTTP client
  - `async-trait` - async traits
  - `tracing` - structured logging (if needed)