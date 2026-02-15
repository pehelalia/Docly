# Docly -- Revised Requirements Document (Aligned with Repository Implementation)

## Introduction

Docly is a Node.js-based CLI tool that generates AI-powered
documentation for codebases using Google's Gemini API. It is executed
via a command-line interface and processes
project files to generate Markdown documentation inside a specified
output directory.

Docly performs the following operations:

-   Parses CLI arguments\
-   Validates input paths\
-   Analyzes project structure\
-   Sends structured prompts to Gemini\
-   Writes generated markdown files to disk

------------------------------------------------------------------------

## Glossary

-   **Docly CLI** -- The executable command defined in `bin/index.js`
-   **CLI Parser** -- Argument parsing logic used in the entry script
-   **Gemini API Client** -- Google Generative AI integration
-   **Project Scanner** -- File system traversal logic
-   **Markdown Generator** -- AI prompt + response handling logic
-   **Output Manager** -- File writing utilities
-   **User** -- Developer running `docly`
-   **Target Project** -- Codebase passed as path argument

------------------------------------------------------------------------

# Functional Requirements

## Requirement 1: CLI Command Execution

### User Story

As a developer, I want to run Docly from the terminal so I can generate
documentation easily.

### Acceptance Criteria

1.  WHEN the user runs the `docly` command, THE CLI SHALL execute via
    `bin/index.js`.
2.  WHEN a project path is provided, THE CLI SHALL validate that the
    directory exists.
3.  WHEN no path is provided, THE CLI SHALL default to the current
    working directory.
4.  WHEN invalid arguments are passed, THE CLI SHALL display usage
    instructions.
5.  THE CLI SHALL read environment variables (including
    `GEMINI_API_KEY`).
6.  THE CLI SHALL gracefully exit on fatal errors.

------------------------------------------------------------------------

## Requirement 2: Project Scanning

### User Story

As a developer, I want Docly to inspect my project files so that the
documentation reflects my codebase.

### Acceptance Criteria

1.  THE system SHALL recursively scan the target directory.
2.  THE system SHALL read supported source code files.
3.  THE system SHALL ignore:
    -   `node_modules`
    -   `.git`
    -   build/dist folders
4.  THE system SHALL gather file contents into structured input for AI
    prompting.
5.  THE system SHALL limit file size or truncate large files to prevent
    token overflow.

------------------------------------------------------------------------

## Requirement 3: AI-Based Documentation Generation

### User Story

As a developer, I want documentation automatically generated using
Gemini AI.

### Acceptance Criteria

1.  THE CLI SHALL send collected project data to Gemini API.
2.  THE CLI SHALL construct a structured prompt instructing Gemini to
    generate documentation.
3.  THE system SHALL receive Markdown-formatted output.
4.  IF the API key is missing, THE CLI SHALL show a clear error.
5.  IF the API call fails, THE CLI SHALL display the error and exit.
6.  THE generated documentation SHALL be saved as Markdown files.

------------------------------------------------------------------------

## Requirement 4: Documentation Output

### User Story

As a developer, I want generated files saved to disk automatically.

### Acceptance Criteria

1.  THE CLI SHALL create a `docs` directory if it does not exist.
2.  THE CLI SHALL write Markdown output files inside that directory.
3.  THE CLI SHALL overwrite existing files without interactive prompts
    (current behavior).
4.  THE CLI SHALL log successful file creation.
5.  IF file writing fails, THE CLI SHALL display an error.

------------------------------------------------------------------------

## Requirement 5: Configuration

### User Story

As a developer, I want to configure Docly via environment variables.

### Acceptance Criteria

1.  THE Gemini API key SHALL be read from `process.env.GEMINI_API_KEY`.
2.  IF the key is missing, execution SHALL stop.
3.  CLI arguments SHALL override defaults.
4.  No `.doclyrc` configuration file is currently required.

------------------------------------------------------------------------

## Requirement 6: Diagram Support

### User Story

As a developer, I want diagram content generated when applicable.

### Acceptance Criteria

1.  IF diagrams are generated, THEY SHALL be returned in Mermaid
    markdown format.
2.  THE CLI SHALL save Mermaid diagrams as `.md` or `.mmd` files.
3.  PNG rendering is NOT guaranteed unless explicitly implemented.

------------------------------------------------------------------------

## Requirement 7: Error Handling

### Acceptance Criteria

1.  IF `GEMINI_API_KEY` is missing, THE CLI SHALL display configuration
    instructions.
2.  IF the project path does not exist, THE CLI SHALL display an error.
3.  IF Gemini API request fails, THE CLI SHALL log the failure.
4.  THE CLI SHALL exit with a non-zero exit code on fatal failure.

------------------------------------------------------------------------

## Requirement 8: CLI Experience

### Acceptance Criteria

1.  THE CLI SHALL display informational logs during execution.
2.  THE CLI SHALL show success messages after generation.
3.  THE CLI SHALL display errors in readable format.
4.  Advanced spinners, colored banners, and time-elapsed tracking are
    optional and dependent on implementation.

------------------------------------------------------------------------

# Summary

This revised document reflects the current implementation structure of
the Docly repository, which includes:

-   A single CLI entry (`bin/index.js`)
-   AI-based documentation generation via Gemini
-   Basic directory scanning
-   Markdown file output
-   Environment variable configuration

It removes advanced features not currently implemented in the
repository.
