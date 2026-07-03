<RULE[project]>
# Logging Guidelines

When working on `elpring-di`, `elpring-web`, or `elpring-boot`, you must follow these logging guidelines:

1. **Dependencies**: Use `org.slf4j:slf4j-api` exclusively for framework logging.
2. **Performance**: Do not use string concatenation (`+`) for logging. Always use slf4j placeholders (`{}`).
3. **No `System.out.println`**: Never use `System.out.println`, `System.err.println`, or `e.printStackTrace()` in the runtime framework code. Use the slf4j `log` instance.
4. **Log Levels**:
   - `INFO`: For framework lifecycle milestones and summaries (e.g. initialization time, started port, number of beans).
   - `DEBUG`: For tracing normal execution flows and troubleshooting (e.g. scanned components, instantiation starts/ends).
   - `TRACE`: For deep diving into detailed algorithms (e.g. dependency resolution flow, `@Primary` selection, generic collection matching).
   - `WARN`/`ERROR`: For exceptions, circular dependencies, port binding failures, or non-fatal configuration issues.
5. **Logger Declaration**: Always declare the logger as `private static final Logger log = LoggerFactory.getLogger(YourClass.class);`
</RULE[project]>
