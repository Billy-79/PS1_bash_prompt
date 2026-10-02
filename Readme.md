# Dynamic Multi-Line Bash Prompt

A lightweight, pure-Bash custom prompt featuring command execution timing, smart Git integration, Python virtual environment detection, and terminal shell integration—all with zero external binary dependencies.

## Highlights & Features

* **⚡ Pure Bash & Lightning Fast:** No Rust, Go, or Python dependencies required. Runs purely on native Bash builtins and lightweight git utilities.
* **⏱ Command Execution Timer:** Tracks and displays how long long-running commands take (`≥ 1s`) in real-time.
* **🔀 Smart Git Status:** Uses fast `git status --porcelain=v2` to render branch info, active operation states (`REBASE`, `MERGE`, `CHERRY-PICK`), uncommitted changes (`+`, `*`, `?`, `✔`), and upstream sync offsets (`⇡ahead`, `⇣behind`).
* **🐍 Python Virtual Environment Integration:** Automatically displays the active `venv` name on a dedicated line without cluttering your primary input row.
* **🛡 Native Readline Safety:** Fully wrapped ANSI sequences to prevent character offset and line-wrapping glitches when typing long commands.
* **🖥 Shell Integration Ready:** Built-in Final Term Command Sequences (FTCS `OSC 133`) for modern terminals (WezTerm, iTerm2, VS Code, GNOME Terminal).

---

## Installation

1. Clone the repository or download the script:
   ```bash
   git clone https://github.com/Billy-79/PS1_bash_prompt.git
   cd PS1_bash_prompt

2. Append the contents of prompt.sh to your ~/.bashrc file:
   ```bash
   cat PS1_bash_prompt >> ~/.bashrc

3. Reload your terminal configuration:
   ```bash
   source ~/.bashrc

---

## Breakdown of the Prompt Structure

The prompt dynamically expands across multiple rows based on your current workspace state:

╭─(HH:MM:SS)-(jobs)-(hostname)-(cwd) (⏱ duration)
├─(git:branch state status sync)
├─(venv:name)
╰─❯_

1. Header Line (Primary Status)

   • Time: 24-hour clock format.

   • Jobs: Active background job counter.

   • Hostname: System host name.

   • Path: Current working directory (~ shortened).

   • Timer: Rendered dynamically in orange when a command takes 1 second or longer.

2. Git Row (Conditional):

   • Rendered only inside Git repositories.

   • Clean checkmark (✔) when working directory is clean.

   • Indicators: + (staged), * (modified), ? (untracked).

   • Sync counters: ⇡1 (ahead by 1 commit), ⇣2 (behind by 2 commits).

3. Virtualenv Row (Conditional):

   • Appears automatically when a Python virtual environment is activated.

4. Input Row:

   • Clean ╰─❯_ line for command execution.

---

## Customization

Palette & Customization Guide

Colors are implemented using 256-color ANSI escape sequences (\e[38;5;<COLOR>m). You can customize the look by tweaking the code values inside PS1_bash_prompt:

Component           Color Name      ANSI Code   Visual Role
Structure           Medium Green    35          Box borders (╭─, ├─, ╰─) and brackets
Metadata	            Bright Cyan     38          Time, background jobs, hostname, path, input cursor (❯_)
Git Branch          Dark Blue       32          Branch names and git label (git:master)
Alerts / Timer      Vibrant Orange  208         Command duration (⏱ 2s), dirty state flags (+*?), venv label
Git Sync            	Warm Yellow     220         Remote ahead/behind counts (⇡1, ⇣2)

---

## Troubleshooting & Verification

   • Line-wrapping or history scroll issues? Ensure your terminal emulator supports
   standard 256-color ANSI codes and UTF-8 characters (╭, ╰, ├, ❯, ⏱, ✔, ⇡, ⇣).

   • Python virtual environment not updating? The script sets export
   VIRTUAL_ENV_DISABLE_PROMPT=1 so that default activate scripts do not overwrite your
   prompt formatting.
