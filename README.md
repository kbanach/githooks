# Git-Hooks

### First line of new commit from last commit

**IMPORTANT**: Linux only

If you're using for every commit the same header (e.g. with current task name, like `BSD-123 - Change in ABC`)
you might need a git hook helper to do that:

* create file `.git/hooks/prepare-commit-msg`:
    ```bash
    $ touch .git/hooks/prepare-commit-msg
    ```

* add content with your favourite editor to this file, e.g. `$ gedit .git/hooks/prepare-commit-msg`:
    ```bash
    #!/bin/sh
    ORIG_MSG_FILE="$1"                   # Grab the current template
    TMP=`mktemp msg-XXXXX --tmpdir="."`  # Create a temp file
    trap "rm -f $TMP" exit               # Remove temp file on exit
    
    MSG=`git log -1 --pretty=%s`         # Grab the first line of the last commit message
    
    # Print all to temp file
    (printf "# Last used commit header \n%s \n\n" "$MSG"; cat "$ORIG_MSG_FILE") > "$TMP" 
    
    cat "$TMP" > "$ORIG_MSG_FILE"        # Move temp file to commit message
    ```
    
* give permission to execute the file:
    ```bash
    $ chmod +x .git/hooks/prepare-commit-msg
    ```
    
* check does it work, by simply running
    ```bash
    $ git add . && git commit
    ```
    
### Useful commit template

If you would like to follow [Semantic Commit Messages](https://seesparkbox.com/foundry/semantic_commit_messages) 
but you don't remember all those `chore`, `docs`, `style` etc. keys, maybe you find a comment note with them
in every new commit message useful:

* create a global git commit message template file somewhere where you like, e.g. in your home directory:
    ```bash
    $ touch ~/.gitmessage.template.txt
    ```
    
* paste some handy cheat sheet with your favourite editor to this file, e.g. `$ gedit ~/.gitmessage.template.txt`:
    ```
    # --- COMMIT END ---
    # Type can be
    #    feat     (new feature)
    #    fix      (bug fix)
    #    refactor (refactoring production code)
    #    style    (formatting, missing semi colons, etc; no code change)
    #    docs     (changes to documentation)
    #    test     (adding or refactoring tests; no production code change)
    #    chore    (updating grunt tasks etc; no production code change)
    # --------------------
    # Remember to
    #    Capitalize the subject line
    #    Use the imperative mood in the subject line
    #    Do not end the subject line with a period
    #    Separate subject from body with a blank line
    #    Use the body to explain what and why vs. how
    #    Can use multiple lines with "-" for bullet points in body
    # --------------------
    ``` 
    
    or (based on [GitCommitEmoji|https://gist.github.com/parmentf/035de27d6ed1dce0b36a] ):
    
    **IMPORTANT**: table column lengths are optimised for Mac iTerm with Monaco 14pt font, so at your terminal configuration you might have an issue with equal columns (emojis don't know how to length properly ;) )
    
    ```
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Initial commit                 | Version tag                    | New feature                    |
    # | 🎉 :tada:                      | 🔖 :bookmark:                  | ✨ :sparkles:                  |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Bugfix                         | Metadata                       | Documentation                  |
    # | 🐛 :bug:                       | 📇 :card_index:                | 📚 :books:                     |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Documenting source code        | Performance                    | Cosmetic                       |
    # | 💡 :bulb:                      | 🐎 :racehorse:                 | 💄 :lipstick:                  |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Tests                          | Adding a test                  | General update                 |
    # | 🚨 :rotating_light:            | ✅ :white_check_mark:          | ⚡️ :zap:                       |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Improve format/structure       | Refactor code                  | Removing code/files            |
    # | 🎨 :art:                       | 🔨 :hammer:                    | 🔥 :fire:                      |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Continuous Integration         | Security                       | Upgrading dependencies         |
    # | 💚 :green_heart:               | 🔒 :lock:                      | ⬆️  :arrow_up:                  |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Downgrading dependencies       | Lint                           | Translation                    |
    # | ⬇️ :arrow_down:                 | 👕 :shirt:                     | 👽 :alien:                     |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Text                           | Critical hotfix                | Deploying stuff                |
    # | 📝 :pencil:                    | 🚑 :ambulance:                 | 🚀 :rocket:                    |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Fixing on MacOS                | Fixing on Linux                | Fixing on Windows              |
    # | 🍎 :apple:                     | 🐧 :penguin:                   | 🏁 :checkered_flag:            |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Work in progress               | Adding CI build system         | Analytics or tracking code     |
    # | 🚧 :construction:              | 👷 :construction_worker:       | 📈 :chart_with_upwards_trend:  |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Removing a dependency          | Adding a dependency            | Docker                         |
    # | ➖ :heavy_minus_sign:          | ➕ :heavy_plus_sign:           | 🐳 :whale:                     |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Configuration files            | Package.json in JS             | Merging branches               |
    # | 🔧 :wrench:                    | 📦 :package:                   | 🔀 :twisted_rightwards_arrows: |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Bad code / need improv.        | Reverting changes              | Breaking changes               |
    # | 💩 :hankey:                    | ⏪ :rewind:                    | 💥 :boom:                      |
    # | ------------------------------ | ------------------------------ | ------------------------------ |
    # | Code review changes            | Accessibility                  | Move/rename repository         |
    # | 👌 :ok_hand:                   | ♿️ :wheelchair:                | 🚚 :truck:                     |
    # | ------------------------------ | ------------------------------ | ------------------------------ |  
    ```
    
* inform git that you want to use it as your main commit template:
    ```bash
    $ git config --global commit.template ~/.gitmessage.template.txt
    ```
    
* that's it! check does it work, by simply running
    ```bash
    $ git add . && git commit
    ```
