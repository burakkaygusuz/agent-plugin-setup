# Project structure

A plugin project has one plugin. The plugin has one or more skills. Each skill can have an agent file. The plugin works with GitHub Copilot and Claude Code.

The project has four parts:

- Skills. Each skill has a `scripts/` folder, a `references/` folder, and an `assets/` folder. A folder that the skill does not need can be omitted.
- Agent files for Claude Code and for Copilot.
- Eval files for each skill.
- Eval files for the full plugin.

The two eval systems use different file formats. Neither tool reads the files of the other tool.

The scripts use Node.js 22.12 or later. The project has no Python code and no packages.

Each client reads these paths:

- Agent Plugins clients and Copilot read `plugin.json`, `skills/`, `mcp.json`, and `com.github.copilot/`.
- Claude Code reads `.claude-plugin/`, `skills/`, `agents/`, and `.mcp.json`.
- Both clients read `.claude-plugin/marketplace.json`.
- The `evals/` folder at the root is for `claude plugin eval`. The folders `skills/<skill-name>/evals/` are for the Agent Skills eval format.

In the tree, `<skill-name>` is the name of one skill. Repeat the folder for each skill. `<agent-name>` is the name of one agent. `<script-name>` and `<topic>` are names that you choose.

```txt
<plugin-name>/
├── plugin.json                        # Agent Plugins 1.0 manifest
├── .claude-plugin/
│   ├── plugin.json                    # Claude Code manifest
│   └── marketplace.json               # marketplace catalog, read by both clients
│
├── skills/                            # both formats read this folder
│   └── <skill-name>/                  # folder name = `name` in SKILL.md
│       ├── SKILL.md                   # under 500 lines
│       ├── scripts/
│       │   └── <script-name>.mjs      # shebang, --help, JSON output
│       ├── references/
│       │   ├── role.md                # role text, single source for the agent body
│       │   └── <topic>.md             # read only when the skill needs it
│       ├── assets/                    # templates and static files
│       │   └── <template>.md
│       └── evals/                     # Agent Skills eval format
│           ├── evals.json             # test cases and assertions
│           ├── train_queries.json     # trigger queries, used to tune it
│           ├── validation_queries.json # different queries, used to check it
│           └── files/                 # input files for the test cases
│               └── <input-file>
│
├── agents/                            # Claude Code agents
│   └── <agent-name>.md                # frontmatter + body from role.md
├── com.github.copilot/                # Copilot-only files
│   └── agents/
│       └── <agent-name>.agent.md      # frontmatter + body from role.md
├── mcp.json                           # optional, Agent Plugins MCP file
├── .mcp.json                          # optional, Claude Code MCP file
│
├── evals/                             # claude plugin eval format
│   ├── <skill-name>-task/
│   │   ├── prompt.md                  # request, as a user types it
│   │   ├── case.yaml                  # case settings
│   │   ├── fixture.sh                 # Bash script, creates the input files
│   │   └── graders/
│   │       ├── criteria.md            # llm grader, gives the score
│   │       └── skill-fired.md         # indicator: did the skill start
│   ├── unrelated-request/             # negative control
│   │   ├── prompt.md
│   │   └── graders/
│   │       └── no-skill.md            # passes if no skill starts
│   └── results/                       # each run writes here
│
├── scripts/
│   └── sync-agents.sh                 # copies role.md into agent bodies; --check
├── .gitignore                         # evals/results/ and .workspace/
├── README.md
├── CHANGELOG.md                       # one entry for each release
└── LICENSE
```
