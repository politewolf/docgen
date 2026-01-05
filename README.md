## Open Source Knowledge Base AI

OSDoc Generator is an intelligent tool that analyzes your codebase and generates comprehensive, accurate architectural documentation automatically. It supports **any programming language** and uses AI-powered agents to understand your project structure, dependencies, patterns, security, and data flows.

---

## ✨ Features

- 🤖 **8 Specialized AI Agents**: File Structure, Dependencies, Patterns, Flows, Schemas, Architecture, Security, and **Repository KPI** (NEW!).
- 🔍 **RAG-Powered Queries**: Query your architecture docs with natural language using FREE local embeddings (TF-IDF + Graph-based retrieval).
- 📊 **Repository Health Dashboard**: LLM-powered KPI analysis with actionable insights on code quality, testing, architecture health, and technical debt.
- 🔍 **RAG Vector Search + Hybrid Retrieval**: Semantic similarity search (FREE local TF-IDF or cloud providers) combined with dependency graph analysis - finds files by meaning AND structure. [See docs →](docs/VECTOR_SEARCH.md)
- 💾 **JSON-First Architecture** (v0.3.37+): All agent outputs stored as JSON with Markdown as rendered views—enable fast local queries, multi-format exports, and zero-LLM-cost lookups.
- ⚡ **Generation Performance Metrics**: Track agent execution times, token usage, costs, and confidence scores in metadata.
- 🌍 **17 Languages Out-of-the-Box**: TypeScript, Python, Java, Go, C#, C/C++, Kotlin, PHP, Ruby, Rust, Scala, Swift, CSS, HTML, JSON, XML, Flex/ActionScript.
- 🧠 **AI-Powered**: Uses LangChain with Claude 4.5, OpenAI o1/GPT-4o, Gemini 2.5, or Grok 3.
- 📚 **Comprehensive Analysis**: Structure, dependencies, patterns, flows, schemas, security, and executive-level KPIs.
- 📝 **Markdown Output**: Clean, version-controllable documentation with smart navigation and dynamic table of contents.
- 🔄 **Iterative Refinement**: Self-improving analysis with quality checks and gap detection (LangGraph-based workflow).
- 🎨 **Customizable**: Prompt-based agent selection and configuration without code changes.
- 📊 **LangSmith Tracing**: Full observability of AI workflows with detailed token tracking and multi-step traces.
- 🔒 **Security Analysis**: Vulnerability detection, authentication review, and crypto analysis.
- ➕ **Extensible**: Add support for any language via configuration—no code changes required.
- 💰 **Delta Analysis** (v0.3.37+): Automatic change detection reduces costs by 60-90% on incremental runs. Uses Git or file hashing to only analyze changed files.
- 🔄 **MCP Integration** (v0.3.30+): Native Model Context Protocol support for Cursor, Claude Code, VS Code + Copilot, and Claude Desktop—access as a native tool in your AI assistant.
- 📂 **Recursive .gitignore Support**: Automatically loads `.gitignore` patterns from any directory level (root and subdirectories), ensuring consistent file filtering across monorepo structures and nested projects.
- 🌿 **Branch-Based Augmented Generation** (v0.3.37+): Delta analysis supports Git branches, tags, and commits—compare against `main`, `develop`, or any specific reference to generate augmented documentation focusing only on changes.

## 🤖 Available Agents

Each agent specializes in a specific analysis task using LLM-powered intelligence:

| Agent                     | Purpose                                    | Priority    | Output File         | Notes                           |
| ------------------------- | ------------------------------------------ | ----------- | ------------------- | ------------------------------- |
| **File Structure**        | Project organization, entry points         | HIGH        | `file-structure.md` | Always runs                     |
| **Dependency Analyzer**   | External deps, internal imports            | HIGH        | `dependencies.md`   | Always runs                     |
| **Architecture Analyzer** | High-level design, components              | HIGH        | `architecture.md`   | Always runs                     |
| **Pattern Detector**      | Design patterns, anti-patterns             | MEDIUM      | `patterns.md`       | Always runs                     |
| **Flow Visualization**    | Control & data flows with diagrams         | MEDIUM      | `flows.md`          | Always runs                     |
| **Schema Generator**      | Data models, interfaces, type definitions  | MEDIUM      | `schemas.md`        | **Only if schemas detected** ⚠️ |
| **Security Analyzer**     | Vulnerabilities, auth, secrets, crypto     | MEDIUM      | `security.md`       | Always runs                     |
| **KPI Analyzer** ⭐ NEW   | Repository health, executive KPI dashboard | MEDIUM-HIGH | `kpi.md`            | Always runs                     |

**⚠️ Schema Generator Smart Behavior:**

The Schema Generator agent is **intelligent** - it only generates output when it detects actual schema files:

**Detects:**

- ✅ **Database**: Prisma schemas (`.prisma`), TypeORM entities (`@Entity`), Sequelize models
- ✅ **API**: DTOs (`.dto.ts`), OpenAPI/Swagger definitions
- ✅ **GraphQL**: Type definitions (`.graphql`, `.gql`)
- ✅ **Types**: TypeScript interfaces, type definitions (focused schema files only)

**Behavior:**

- If **NO schemas found**: Generates `schemas.md` with "No schema definitions found" message
- If **schemas found**: Generates comprehensive documentation with Mermaid ER/class diagrams
- Uses `__FORCE_STOP__` to avoid unnecessary LLM calls when no schemas exist

**Why "No schemas"?**

- Project may use embedded types in service/controller files (not dedicated schema files)
- Database-less projects (e.g., static site generators, CLI tools)
- API-only projects using inline interfaces

This is **not a failure** - it's smart detection saving you tokens and cost! 💰

**KPI Analyzer Features:**

- 📊 Overall repository health score (0-100%)
- 🎯 Component scores: Code quality, testing, architecture, dependencies, complexity
- 📈 Detailed metrics with ASCII visualizations
- 💡 8+ actionable insights with prioritized action items
- 🚀 Executive-friendly language with quantifiable targets

## 🏗️ Architecture Highlights

### Multi-Agent System

The orchestrator coordinates agents to perform analysis.

```
┌─────────────────────────────┐
│  Documentation Orchestrator │
└─────────────┬─────────────┘
              │
    ┌─────────┴─────────┐
    │  Agent Registry   │
    └─────────┬─────────┘
              │
┌───▼────┐  ┌───▼───┐  ┌───▼───┐
│ Agent 1│  │ Agent 2│  │ Agent N│
└────────┘  └───────┘  └───────┘
```

### Self-Refining Analysis

Each agent autonomously improves its analysis through iterative refinement. It evaluates its own output, identifies gaps, searches for relevant code, and refines until quality thresholds are met.

**[Learn how the self-refinement workflow works →](./docs/AGENT_WORKFLOW.md)**

### LangChain LCEL Integration

All agents use LangChain Expression Language (LCEL) for composable AI workflows with unified LangSmith tracing.

## 📊 Language Support

This project supports **17 programming and markup languages** out-of-the-box with **zero configuration**:

### Programming Languages

| Language                  | Extensions                                       | Import Detection              | Framework Support                             |
| ------------------------- | ------------------------------------------------ | ----------------------------- | --------------------------------------------- |
| **TypeScript/JavaScript** | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`, `.cjs`     | ES6 imports, CommonJS require | NestJS, Express, React, Angular, Vue, Next.js |
| **Python**                | `.py`, `.pyi`, `.pyx`                            | `from...import`, `import`     | Django, Flask, FastAPI, Pyramid               |
| **Java**                  | `.java`                                          | `import` statements           | Spring Boot, Quarkus, Micronaut               |
| **Go**                    | `.go`                                            | `import` blocks               | Gin, Echo, Fiber, Chi                         |
| **C#**                    | `.cs`, `.csx`                                    | `using` statements            | ASP.NET, Entity Framework                     |
| **C/C++**                 | `.c`, `.cpp`, `.cc`, `.cxx`, `.h`, `.hpp`, `.hh` | `#include` directives         | Linux, POSIX                                  |
| **Kotlin**                | `.kt`, `.kts`                                    | `import` statements           | Spring, Ktor, Micronaut                       |
| **PHP**                   | `.php`                                           | `use`, `require`              | Laravel, Symfony                              |
| **Ruby**                  | `.rb`, `.rake`                                   | `require` statements          | Rails, Sinatra                                |
| **Rust**                  | `.rs`                                            | `use` statements              | Tokio, Actix, Rocket                          |
| **Scala**                 | `.scala`                                         | `import` statements           | Akka, Play                                    |
| **Swift**                 | `.swift`                                         | `import` statements           | SwiftUI, Vapor                                |

### Web & Data Languages

| Language              | Extensions               | Detection                | Notes                        |
| --------------------- | ------------------------ | ------------------------ | ---------------------------- |
| **CSS**               | `.css`, `.scss`, `.sass` | `@import` rules          | Theme and variable detection |
| **HTML**              | `.html`, `.htm`          | `src`, `href` attributes | Script/link/image extraction |
| **JSON**              | `.json`                  | N/A                      | Configuration file analysis  |
| **XML**               | `.xml`                   | `xi:include` elements    | XInclude support             |
| **Flex/ActionScript** | `.as`, `.mxml`           | `import` statements      | Flash/Flex project support   |


### Custom Language Support

Need support for a language not listed? **No code changes required!**

Add custom language configurations via `.archdoc.config.json`:

```json
{
  "languages": {
    "custom": {
      "myLanguage": {
        "displayName": "My Language",
        "filePatterns": {
          "extensions": [".mylang"]
        },
        "importPatterns": {
          "myImport": "^import\\s+([^;]+);"
        }
      }
    }
  }
}
```
