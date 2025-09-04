# Claude Code - Commands

## Context Management

### Clearing Context During Session

- **`/exit`** - Exits the chat session completely
- **`/clear`** - Clears the entire session context and chat history
- **`/compact`** - Compacts the chat & context into a small summary
- **`ESC ESC`** - Rewind to a previous point in the session (press ESC twice)
- **`/resume`** - Resume a previous chat session
### Multi-file Operations

- **`/diff`** - Show differences between file versions
- **`/status`** - Check project status and recent changes
- **`/commit`** - Commit changes with AI-generated message

### Available Thinking Modes

- **`think`** - Basic extended thinking
- **`think hard`** - Increased thinking budget
- **`think harder`** - Even more computational power
- **`ultrathink`** - Maximum thinking allocation

#### Usage Examples

```
User: think about the best architecture for this microservice
User: think hard about optimizing this database query
User: think harder about this complex algorithm design
User: ultrathink about the entire system architecture refactoring
```

## Custom Commands

### Creating Custom Commands

Custom commands are defined in markdown files inside the `.claude/commands/` folder in your project root.

### Command File Structure

Each command file follows this YAML frontmatter + markdown format:

```markdown
---
description: Brief description of what the command does
argument-hint: Expected arguments format
---

## Context
Instructions for parsing arguments and extracting values.

Parse $ARGUMENTS to get the following values:
- [variable1]: Description of first variable from $ARGUMENTS
- [variable2]: Description of second variable from $ARGUMENTS

## Task
Detailed instructions for Claude on how to execute this command.

Make sure to reference the parsed variables using [variable1] and [variable2] syntax.
```

### Real Example: UI Component Command

Create `.claude/commands/ui-component.md`:

```markdown
---
description: Create a UI Component in /components/ui
argument-hint: Component name | Component summary
---

## Context

Parse $ARGUMENTS to get the following values:
- [name]: Component name from $ARGUMENTS, converted to PascalCase
- [summary]: Component summary from $ARGUMENTS

## Task

Make a UI component according to the [name] and [summary] provided, following these guidelines:

- Create the component file in `src/components/ui/[name]/[name].tsx`
- Use a functional component with the name [name]
- Reference the [summary] when making the component
```

# Claude Code - Useful MCP Servers

## Recommended MCP Servers

### Context7

**Function**: Access to up-to-date documentation for libraries and frameworks

- **Repository**: [upstash/context7](https://github.com/upstash/context7)
- **Use case**: Always check the most recent documentation when implementing new libraries

### Playwright

**Function**: Browser interaction and automation

- **Repository**: [microdoft/playwright-mcp](https://github.com/microsoft/playwright-mcp)
- **Use case**: Automated testing, web scraping, UI automation

## 💡 Claude Memory Tip

Add to Claude's memory:

```
#Use Context7 to check up-to-date docs when needed for implementing new libraries or frameworks, or adding features using them.
```

This ensures Claude always checks the latest version of documentation before suggesting implementations.

# Claude Code - Creating Subagents

## What are Subagents?

Subagents are specialized AI assistants that can be invoked to handle specific types of tasks. They enable more efficient problem-solving by providing task-specific configurations with customized system prompts, tools and a separate context window.

## Creating a Subagent

### Using the `/agents` Command (Recommended)

The `/agents` command provides a comprehensive interface for subagent management:

```bash
/agents
```

This opens an interactive menu where you can:

- Create new subagents with guided setup
- Edit existing subagents and their tool access
- View all available subagents
- Manage tool permissions

### Manual Creation

Subagents are stored as Markdown files with YAML frontmatter in:

- **Project level**: `.claude/agents/` (highest priority)
- **User level**: `~/.claude/agents/` (available across all projects)

## File Format

```markdown
---
name: ux-reviewer
description: Expert UI & UX engineer who reviews the UI & UX of React components in a browser using Playwright, takes screenshots, then offers feedback on how to improve the component in terms of visual design, user experience and accessibility
tools: Playwright, Read, Write
---

You are an expert UI/UX engineer specialized in reviewing React components through browser testing.

When invoked:
1. Use Playwright to open the component in a browser
2. Take comprehensive screenshots of different states
3. Analyze visual design, user experience, and accessibility
4. Provide specific, actionable feedback
5. Suggest concrete improvements

Key areas to focus on:
- Visual hierarchy and layout
- Color contrast and accessibility (WCAG compliance)
- Responsive design across breakpoints
- User interaction patterns and affordances
- Component state feedback (loading, error, success)
- Typography and spacing consistency

Always provide both positive observations and specific improvement suggestions with clear implementation guidance.
```

## Configuration Options

### Available Fields

Each subagent configuration includes these fields:

|Field|Required|Description|
|---|---|---|
|`name`|Yes|Unique identifier using lowercase letters and hyphens|
|`description`|Yes|Natural language description of when this agent should be used|
|`tools`|No|Comma-separated list of specific tools (omit to inherit all tools)|

### Tool Configuration

You have two options for configuring tools:

- **Omit the `tools` field**: Inherits all tools from main thread (including MCP tools)
- **Specify individual tools**: Comma-separated list for granular control

## Usage

### Automatic Delegation

Claude Code proactively delegates tasks based on the task description in your request, the description field in subagent configurations, and current context.

### Explicit Invocation

```bash
# Request specific subagent
Use the ux-reviewer agent to analyze this login component

# General request (automatic delegation)
Review the UI of my new dashboard component for accessibility issues
```

## Best Practices

1. **Start with Claude-generated agents: Generate your initial subagent with Claude and then iterate on it to make it personally yours**
    
2. **Design focused subagents: Create subagents with single, clear responsibilities rather than trying to make one subagent do everything**
    
3. **Write detailed prompts: Include specific instructions, examples, and constraints in your system prompts**
    
4. **Limit tool access: Only grant tools that are necessary for the subagent's purpose**
    

## Documentation Reference

For complete documentation: [Claude Code Subagents](https://docs.anthropic.com/en/docs/claude-code/sub-agents)