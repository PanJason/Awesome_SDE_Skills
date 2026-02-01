---
name: frontend-impl
description: Implement frontend components from DESIGN.md with well-formatted commits and documentation
metadata:
  style: Precise
  audience: frontend developers
  platforms: SwiftUI, UIKit, React, Vue, Angular
---

# Frontend Implementation Skill

Implements frontend components from technical design documents (**DESIGN.md**)
with well-structured git commits and comprehensive documentation following Linux
kernel-doc conventions.

## Usage
```
/frontend-impl <component-name>
```

Example:
```
/frontend-impl pdf-viewer
/frontend-impl chat-panel
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
- Identify the target platform (SwiftUI, UIKit, web framework)

### 2. Implementation Planning
- Break down component into logical implementation units
- Plan commit sequence following dependency order:
  1. Data models and types
  2. Core component structure
  3. Subcomponents and views
  4. Event handlers and business logic
  5. Styling and animations
  6. Integration points
- Separate code commits from documentation commits

### 3. Code Implementation
For each implementation unit:
- Write clean, idiomatic code for the target platform
- Follow platform conventions (SwiftUI property wrappers, React hooks, etc.)
- Include succint error handling and loading states.
- Avoid over engineering on error handling. Focus on the logic of core
  functionalities.
- Add inline comments for complex logic
- Create conventional commit following the `/commit` skill

### 4. Documentation Generation
After code implementation:
- Generate well-formatted documentation in separate `/doc` folder
- Document all public structures, classes, protocols, and functions
- Include parameter descriptions, return values, and examples
- Create separate documentation commit(s)

## Commit Strategy

### Code Commits (Conventional Format)
```
<type>(<scope>): <description>

[detailed changes]

[references]
```

**Types for frontend work:**
- `feat`: New component or feature
- `fix`: Bug fixes
- `refactor`: Code restructuring
- `style`: UI/styling changes
- `perf`: Performance improvements
- `test`: Component tests

**Scopes:**
- Component name (e.g., `pdf-viewer`, `chat-panel`)
- Feature area (e.g., `ui`, `layout`, `state`)

### Documentation Commits (Separate)
```
docs(<component>): add kernel-doc style documentation

- Document public API surface
- Add usage examples
- Include parameter and return value descriptions
```

**Rules:**
- Documentation commits MUST be separate from code commits
- Generate docs AFTER implementation is complete
- One docs commit per component (or logical grouping)

## Documentation Format (Kernel-Doc Style)

### For Swift/Objective-C
```swift
/**
 * struct_name - Brief description
 * @property1: Description of property1
 * @property2: Description of property2
 *
 * Longer description of the structure's purpose,
 * usage patterns, and any important notes.
 *
 * Example:
 *     let instance = StructName(property1: value1)
 *     instance.someMethod()
 */
struct StructName {
    var property1: Type
    var property2: Type
}

/**
 * function_name() - Brief description
 * @param1: Description of param1
 * @param2: Description of param2
 *
 * Return: Description of return value
 *
 * Detailed description of function behavior,
 * side effects, and usage notes.
 *
 * Example:
 *     let result = function_name(param1: "test", param2: 42)
 */
func function_name(param1: String, param2: Int) -> Result {
    // implementation
}
```

### For TypeScript/JavaScript
```typescript
/**
 * ComponentName - Brief description
 * @param {Object} props - Component properties
 * @param {string} props.title - Title to display
 * @param {Function} props.onAction - Callback function
 *
 * Longer description of component purpose,
 * behavior, and usage patterns.
 *
 * Example:
 *     <ComponentName title="Hello" onAction={handleAction} />
 *
 * Return: JSX.Element
 */
export function ComponentName({ title, onAction }: Props): JSX.Element {
    // implementation
}
```

### Documentation Sections
Each component documentation needs to include the following:

1. **Component Overview Description**
   - Purpose
   - Position in architecture
   - Key features

2. **Public API** (Optional, NECESSARY if there are public APIs exposed)
   - All exported types, interfaces, classes
   - Public functions and methods
   - Props/parameters with types

3. **State Management** (Optional, NECESSARY if the method or function is stateful)
   - Internal state description
   - State update patterns
   - Side effects

4. **Integration Points**
   - Dependencies on other components
   - Events emitted
   - Callbacks expected
   - Visible behavior over external data

5. **Usage Examples**
   - Basic usage
   - Common patterns

## Platform-Specific Guidelines

### SwiftUI Components
- Use `@State`, `@Binding`, `@ObservedObject` appropriately
- Follow SwiftUI view composition patterns
- Implement `View` protocol properly
- Use `ViewModifier` for reusable styling
- Document property wrappers and their purpose

### UIKit Components
- Subclass appropriate base classes (`UIView`, `UIViewController`)
- Override lifecycle methods with documentation
- Implement delegates and data sources
- Document outlet connections
- Include Auto Layout constraints explanation

### React Components
- Use functional components with hooks
- Follow React best practices (memoization, effects)
- Implement proper prop validation
- Document hook dependencies
- Include accessibility attributes

### Vue Components
- Use Composition API (preferred) or Options API
- Document reactive properties
- Explain computed properties and watchers
- Include template slot documentation
- Document emitted events

## Example Output

### Commit Sequence for "PDF Viewer Component"
```
1. feat(pdf-viewer): add PDFDocument model and types
2. feat(pdf-viewer): implement PDFView wrapper component
3. feat(pdf-viewer): add text selection handling
4. feat(pdf-viewer): integrate context menu actions
5. style(pdf-viewer): add layout and styling
6. docs(pdf-viewer): add comprehensive API documentation
```

### Sample Documentation Output
```swift
// File: docs/PDFViewer.md

# PDFViewer Component Documentation

## Overview
The PDFViewer component provides a native macOS PDF viewing experience
using PDFKit with text selection and context menu integration.

## Public API

/**
 * PDFViewer - Main PDF viewing component
 * @fileURL: URL of the PDF file to display
 * @onTextSelected: Callback when text is selected
 * @onContextMenuAction: Callback for context menu actions
 *
 * A SwiftUI wrapper around PDFKit's PDFView that provides
 * text selection capabilities and custom context menu actions.
 * Handles PDF loading, rendering, and user interactions.
 *
 * Example:
 *     PDFViewer(
 *         fileURL: documentURL,
 *         onTextSelected: { text in print(text) },
 *         onContextMenuAction: handleAction
 *     )
 *
 * Return: SwiftUI View
 */
struct PDFViewer: View {
    let fileURL: URL
    let onTextSelected: (String) -> Void
    let onContextMenuAction: (ContextAction) -> Void
}

/**
 * currentSelection() - Get currently selected text
 *
 * Return: Optional string containing selected text, nil if no selection
 *
 * Retrieves the current text selection from the PDF view.
 * Returns nil if no text is selected or if the PDF is not loaded.
 *
 * Example:
 *     if let text = viewer.currentSelection() {
 *         print("Selected: \(text)")
 *     }
 */
func currentSelection() -> String? {
    // implementation
}
```

## Quality Standards

### Code Quality
- Follow platform style guides (Swift API Design Guidelines, Airbnb JS Style)
- Maintain consistent naming conventions following [Google Style
  Guides](https://google.github.io/styleguide/) for any language
- Write self-documenting code with clear variable names
- Add comments for non-obvious logic only
- Handle edge cases and errors only necessary. Don't over-engineering on error
  handling.

### Documentation Quality
- Use clear, concise language. Try to be as short as possible.
- Provide realistic examples
- Document all public APIs
- Explain complex algorithms or patterns in succint language.
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
- Flag potential performance or security concerns, but for PoC don't stick too
  much to them

## Deliverables

For each component implementation:
1. Working code committed in logical sequence
2. All public APIs documented in a well-formatted way
3. Separate documentation commit(s)
4. Examples provided in documentation
5. Component integration verified
6. Summary of implementation decisions and tradeoffs in the code commit.

## Integration with Other Components

- Read the doc generated from backend if possible before implementing

## Integration with Other Skills

- Uses `/commit` skill for all git commits
- Can invoke testing skills for component validation

## Notes

- Always read **README.md** and **DESIGN.md** before starting implementation.
  This is possibily gitignored. So use `rg --files --no-ignore`
- Confirm platform/framework before generating code
- Keep code commits and doc commits strictly separated
- Follow existing project structure and conventions
- Update component status (what is implemented and what is WIP) in **IMPL.md**
  when complete. Note that **IMPL.md** is possibly gitignored. So use `rg
  --files --no-ignore`