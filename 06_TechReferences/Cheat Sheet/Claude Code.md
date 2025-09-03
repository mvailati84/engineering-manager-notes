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

