---
name: codex-bot
description: Submit coding and long development tasks to Codex background tools, with persistent results and automatic completion delivery.
---

# Codex coding executor

Prefer `codex_task_submit(project, prompt)` for authorized coding tasks. The installed background-task plugin starts a detached Codex CLI worker, persists its result and sends completion to this chat automatically. Authentication, provider and model are configured. Never inspect or print authentication files.

1. Establish the user's authorized scope. Ask before changing a project unless the task already has explicit authorization or the user's message ends in `1`. Read project AGENT.md and AGENTS.md. Pass the complete request, constraints, source paths and acceptance criteria in `prompt`: the worker does not inherit chat history.
2. Choose an English project ID. Its working directory is `/AstrBot/data/codex-projects/<project>`. Keep each project in its own directory. For an existing project elsewhere, pass its exact source path and authorized scope explicitly; do not claim files were changed in the new directory.
3. Call `codex_task_submit`. Report the returned task ID and submission state. Once accepted, this chat turn may end: the independent monitor sends the final result. Do not claim completion at submission time. Do not poll continuously or run a duplicate shell task.
4. Use `codex_task_status` when asked for progress; an empty task ID lists recent owned tasks in the current chat. Use `codex_task_stop` when asked to stop. File changes already made are retained.
5. Use `codex_task_resume(task_id, prompt)` for follow-up instructions or recovery. A running task must finish or be stopped first. Container restart interrupts workers; inspect the project before resuming. Plugin reload preserves workers. Notification failures are retried; a crash immediately after sending may produce a duplicate notification.

Only AstrBot administrators may use these tools, and status, stop and resume are scoped to the submitting sender and chat. Default concurrency is two tasks, with one active task per project directory. If background tools are unavailable, explain the issue; a shell command alone does not provide independent completion delivery.

Codex runs as root inside the existing AstrBot Docker container with `approval_policy=never` and `sandbox_mode=danger-full-access`. Container restrictions prevent its nested bubblewrap sandbox. It can access container files, network and mounted AstrBot data; project directories are organizational boundaries, not operating-system isolation. Scope all work to the authorized project. Approval prompts cannot be answered interactively in `exec`.

Before a test, import, build, install or generation command, assess file creation and state changes. Commands that can initialize runtime data must run in a temporary directory or isolated copy outside the project; inspect new files afterward. Preserve existing projects' authorship, README and license. Use Chinese in user-facing replies. For new projects use MIT unless specified otherwise, author ahuai and email ahuai@ahuai.top where needed.
