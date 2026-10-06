# Dynamic Multi-Line Bash Prompt

A lightweight, pure-Bash custom prompt featuring command execution timing, smart Git integration, Python virtual environment detection, Debian/Ubuntu chroot monitoring, and terminal shell integration—all with zero external binary dependencies.

## Highlights & Features

* **⚡ Pure Bash & Distribution Agnostic:** No Rust, Go, or Python dependencies required. Built strictly on native Bash builtins and POSIX utilities for 100% universal portability across Arch, Debian, Ubuntu, Fedora, Alpine, and macOS.
* **⏱ Command Execution Timer:** Tracks and displays how long long-running commands take (`≥ 1s`) in real-time.
* **🔀 Smart Git Status:** Uses fast `git status --porcelain=v2` to render branch info, active operation states (`REBASE`, `MERGE`, `CHERRY-PICK`), uncommitted changes (`+`, `*`, `?`, `✔`), and upstream sync offsets (`⇡ahead`, `⇣behind`).
* **📦 Debian/Ubuntu Chroot Detection:** Automatically displays active `/etc/debian_chroot` or `$debian_chroot` environment indicators on supported systems without side effects on non-Debian distributions.
* **🐍 Python Virtual Environment Integration:** Automatically displays the active `venv` name on a dedicated row without cluttering your primary input line.
* **🛡 Native Readline & Globbing Safety:** Fully wrapped ANSI sequences prevent cursor alignment bugs. Uses native Bash `[[ ... ]]` conditional logic to strictly guard against directory globbing leaks when working with wildcards (`*`, `?`).
* **🖥 Shell Integration Ready:** Built-in Final Term Command Sequences (FTCS `OSC 133`) for modern terminal navigation (WezTerm, iTerm2, VS Code, GNOME Terminal, Kitty).

---

## Installation

1. Clone the repository or download the script:
   ```bash
   git clone https://github.com/Billy-79/PS1_bash_prompt.git
   cd PS1_bash_prompt

2. Append the contents of PS1_bash_prompt to your ~/.bashrc file:
   ```bash
   cat PS1_bash_prompt >> ~/.bashrc

3. Reload your terminal configuration:
   ```bash
   source ~/.bashrc

---

## Breakdown of the Prompt Structure

The prompt dynamically expands across multiple rows based on your current workspace state:

```text
╭─(HH:MM:SS)-(jobs)-(user@hostname)-(cwd) (⏱ duration)
├─(chroot:name)
├─(git:branch state status sync)
├─(venv:name)
╰─❯_
```

1. Header Line (Primary Status)

   • Time: 24-hour clock format (HH:MM:SS).

   • Jobs: Count of active background job instances.

   • User & Host: Currently logged-in username and operating system hostname (user@hostname).

   • Path: Current working directory path (~ shortened).

   • Timer: Rendered dynamically in orange when a command execution takes 1 second or longer.
   
2. Debian Chroot Row (Conditional)

   • Renders automatically on Debian/Ubuntu systems when inside an active chroot container (reads $debian_chroot or /etc/debian_chroot).

3. Git Row (Conditional):

   • Rendered only when inside a valid Git repository working tree.

   • Displays clean checkmark (✔) when working tree is unmodified.

   • Uncommitted Flags: + (staged), * (modified), ? (untracked).

   • Sync Counters: ⇡1 (ahead by 1 commit), ⇣2 (behind by 2 commits).

4. Virtualenv Row (Conditional):

   • Appears automatically on its own row whenever a Python virtual environment is active.

5. Input Row:

   • Clean ╰─❯_ line for user command execution and terminal prompt markers (OSC 133;B).

---

## Customization & Color Palette

Colors are implemented using 256-color ANSI escape sequences (`\e[38;5;<COLOR>m`). You can customize the look by tweaking the code values inside `PS1_bash_prompt`:

| Component | Color Name | ANSI Code | Visual Role |
| :--- | :--- | :--- | :--- |
| **Structure** | Medium Green | `35` | Box borders (`╭─`, `├─`, `╰─`) and brackets |
| **Metadata** | Bright Cyan | `38` | Time, background jobs, `user@hostname`, path, input prompt (`❯_`) |
| **Chroot** | Magenta | `201` | Active Debian/Ubuntu chroot context (`chroot:name`) |
| **Git Branch** | Dark Blue | `32` | Branch names and git label (`git:main`) |
| **Alerts / Timer** | Vibrant Orange | `208` | Command duration (`⏱ 2s`), dirty state flags (`+*?`), venv label |
| **Git Sync** | Warm Yellow | `220` | Upstream ahead/behind counts (`⇡1`, `⇣2`) |

---

## Troubleshooting & Verification

   • Testing the Chroot Indicator: You can verify the Debian chroot row in any shell session by running export debian_chroot="test". To remove it, run unset debian_chroot.

   • Line-wrapping or history scroll issues? Ensure your terminal emulator supports standard 256-color ANSI escape codes and UTF-8 box characters (╭, ╰, ├, ❯, ⏱, ✔, ⇡, ⇣).

   • Python virtual environment not updating? The script exports VIRTUAL_ENV_DISABLE_PROMPT=1 so that standard Python activate scripts do not override or corrupt your custom prompt layout.
