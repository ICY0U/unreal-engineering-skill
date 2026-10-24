# Unreal Engineering Skill

A standalone AI skill for Unreal Engine Blueprints, gameplay C++, editor tools,
UMG/Slate, animation, materials, cooking, packaging, and game modding.

It combines a Blueprint craft workflow with Unreal engineering references:
small testable changes, tidy graphs, version-matched research, explicit ownership,
and evidence that distinguishes compilation from real gameplay and visual review.

The skill uses the open [Agent Skills format](https://agentskills.io/specification).
Its instructions are provider-neutral Markdown with relative references. No API
key, paid service, specific model, MCP server, or other skill is required.

Download the [standalone skill ZIP](https://github.com/ICY0U/unreal-engineering-skill/releases/latest/download/unreal-engineering.zip)
or browse the [skill entrypoint](skills/unreal-engineering/SKILL.md).
Extract the ZIP and keep the complete `unreal-engineering` folder together.

## What is included

```text
skills/
  unreal-engineering/
    LICENSE
    SKILL.md
    references/
      blueprints.md
      cpp.md
      ui.md
      animation-and-materials.md
      editor-automation.md
      packaging.md
      modding.md
```

The instructions are engine-version aware. The carried-forward practical lessons
come primarily from UE 5.4 and 5.6 work; they are not a compatibility guarantee for
every engine version, custom fork, game, or loader.

## Use with your agent

### Any Agent Skills-compatible client

Copy the complete `skills/unreal-engineering` folder into the client's documented
skill location. Preserve `SKILL.md` and its `references` directory together.
Ask the agent to use `unreal-engineering` for the task.

Native discovery, invocation syntax, and installation paths belong to the client,
not the underlying language model. Check your client's current documentation.

### Codex

Copy `skills/unreal-engineering` into either:

- `.agents/skills/unreal-engineering` in the target project, for that repository;
- `~/.agents/skills/unreal-engineering`, for your user account.

If that destination already exists, compare or back it up before replacing it.
Keep one intended version active: skills with the same name are not merged.
Invoke it with `$unreal-engineering`, or let Codex select it for a matching task.
See the [official skill documentation](https://learn.chatgpt.com/docs/build-skills).

### A model or chat client without native skill support

Provide `SKILL.md` as task context and attach only the reference files relevant to
your request. For example:

> Follow the attached Unreal Engineering skill. This project uses UE 5.4.4.
> Inspect the existing player input path and fix the reported controller issue.
> Use the UI and Blueprint references. Report which checks you actually ran.

Text instructions can guide a model that reads them. Executing edits, compiling,
controlling the editor, and launching a game still require tools and permission
in that model's host application. This skill supplies the workflow, not an Unreal
connection or a replacement for the editor and build toolchain.

## Example requests

- "Use unreal-engineering to add a reusable Blueprint interaction component."
- "Fix this widget's controller navigation and verify its real entry route."
- "Review this gameplay C++ change for object lifetime and module dependencies."
- "Package this mod for the supplied game build and verify the container contents."
- "Rebuild this animation behavior from the supplied references and identify gaps."

## Contributing

Keep the entrypoint concise and put substantial topic-specific guidance in the
matching reference. Describe an observed failure, its engine/tool version, the
correction, and the check that distinguishes success from failure. Generalize
project incidents before sharing them; keep private paths, credentials, game
assets, and session transcripts outside contributions.

Validate the frontmatter against the Agent Skills specification and check every
relative link. Exercise realistic tasks, including missing editor access and an
unknown mod loader. Report structural validation, simulated task review, and real
Unreal runtime results separately. Cross-model support should only be described
as tested when that model and host have actually been exercised.

## License

MIT; see [LICENSE](LICENSE). The license covers this repository's instructions.
Unreal Engine and third-party tools remain under their respective licenses.
