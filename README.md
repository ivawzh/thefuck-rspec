# thefuck-rspec
Type `fuck` in terminal to re-run the first failing RSpec test from the last time.

![demo](./demo.gif)

## Installation

1. Install thefuck https://github.com/nvbn/thefuck#installation . TheFuck is a terminal tool that corrects errors in previous console commands.

   TLDR:
   ```bash
   brew install thefuck
   echo "eval $(thefuck --alias)" >> ~/.zshrc # or ~/.bashrc if you don't use oh-my-zsh

   # Optional alias to run fixed commands without confirmation

   echo "alias fk='fuck --yeah'" >> ~/.zshrc # or ~/.bashrc if you don't use oh-my-zsh
   ```
2. Add rule for RSpec:

    ```bash
    cat <<FUCKRSPEC >> ~/.config/thefuck/rules/rspec.py
    import re

    def match(command):
        return ('Failed examples:\n\nrspec' in command.output)

    def get_new_command(command):
        rspec_commands = re.findall(r'Failed examples:\n\n(rspec \.\S*_spec\.rb:\d*) #', command.output)
        return f"bundle exec {rspec_commands[0]}"

    enabled_by_default = True

    priority = 1  # Lower first, default is 1000

    requires_output = True
    FUCKRSPEC
    ```
Done. Now you can start using `fk` after opening a new tab in terminal or reloading zsh via `source ~/.zshrc`.
