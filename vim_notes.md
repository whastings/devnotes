# Vim Notes

## File management
* `:e` filename` opens an existing or new file

## Working with Tabs
* In cli `vim -p file1 file2... filen` opens multiple files in tab
* `:tabe` opens a file in a new tab
* `gt` goes forward one tab
* `gT` goes back one tab
* `:tabm position` moves current tab to a given position in the tab order
* `:help tabp` brings up tab help page

## Working with split view
* `:split filename` opens file in a new split window (horizontally)
* `:vsplit filename` opens file in a new split window (vertically)
* `ctrl + w + l` and `ctrl + w +h` move between windows
* `:on` closes all windows except the current window
* `:windo command` will run a command in all open windows

## Text editing
* `:t.` will duplicate a line
* `ctrl + p` will autocomplete a word
* `o` adds a new line below cursor and starts insert there
* `O` adds a new line above cursor and starts insert there
* `gi` goes back to last place you were in insert mode
* `gq` will reformat text
* `gqip` will reformat a paragraph
* `gv` to select last selected area
* `g;` moves to previous change
* `g,` moves to next change
* `ctrl + o n` executes normal mode command *n* from insert mode (good for macros)
* `ctrl + v` enters **Visual Block Mode**.
* `=` auto indents visually selected lines
* `J` will combine the next line with the current line

## Macros
* Creating a macro:
  * Type `q` plus a letter (which register to record to)
  * Type the keystrokes to record
  * Type `q` to stop recording
  * Type `@` plus the letter for the register
  * Type `@@` to re-run the macro
* `:5,10norm! @a` will run macro *a* on lines 5 through 10
* `:%norm! @a` will run macro *a* on all lines
* `:g/pattern/normal! @a` will run macro *a* on all lines matching *pattern*
* `5@a` will run macro *a* five times

## Buffers
* `:ls` to show all buffers

## Registers
* Refer to a register with `"` plus its letter or number
* `"ayy` will copy a line to register `a`
* `"ap` will paste from register `a`
* `"Ayy` will copy a line and *append* it to register `a`
* `:reg` shows the content of all registers
* `:reg num_or_letter` shows contents of a specific register
* The blackhole register is named with an underscore (can truly delete text when
  writing it here)

## Indenting
* `>>` Indents line
* `<<` Unindents line
* `num>>` indents num lines

## Text deleting
* `x` deletes the current character
* `dd` deletes the current line
* `dtCHAR` deletes everything up to the nearest occurrence of CHAR

## Find & replace
* `:%s/search_value/replace_value/g` will replace all occurrences of
  search_value with replace_value
* `q/` brings up a command window with recent searches

## Navigation
* `$` goes to end of line
* `0` (zero) goes to beginning of line
* `w` moves forward a word
* `moves` backward a word
* `L` goes to the bottom of the screen
* `H` goes to the top of the screen
* `}` goes forward one paragraph
* `line_numberG` goes to a specific line number
* `ctrl + b` moves up one page
* `ctrl + f` moves down one page
* `ctrl + e` scrolls down one line
* `ctrl + y` scrolls up one line
* `ctrl + z` to switch back to shell (then `fg` to bring VIM back)
* `ctrl + i` goes to next jump location
* `ctrl + o` goes to previous jump location
* `ctrl + ]` jumps to definition via ctags
* `zz` to center current line in middle of screen

## Cut, copy, & paste
* `v` starts selecting characters
* `V` starts selecting whole lines
* `d` cuts selection
* `y` copies selection
* `p` pastes after cursor
* `P` pastes before cursor
* `VNUMJ` selects NUM lines downward

## Marks
* `m` plus lowercase letter sets mark to that letter for current file.
* `'` plus letter goes to that mark
* `:marks` shows existing marks

## Spell Check
* `zg` adds word to dictionary when it's marked as misspelled.
* `z=` brings up correction suggestions when on misspelled word.
* `]s` goes to next misspelled word.
* `[s` goes to previous misspelled word.

## Commands
* `:so $MYVIMRC` reloads your ~/.vimrc
* `:call ReloadAllSnippets()` reloads all SnipMate snippets
* `:qa` quit all open files
* `:set filetype?` shows the filetype of the currently open file
* `:! shell_command` to execute a shell command from within VIM
* `:SyntasticInfo` to see linter used for current file.
* `q:` brings up a command window with recent commands
* `:cq` force-quits Vim w/ a non-zero status (good for aborting
  process that started Vim).
* `:argdo command` executes command on every open buffer. 
* `:retab` replaces tabs in a file w/ your configured indentation.

## Plugins

### AndrewRadev/splitjoin.vim

* gS to split a one-liner into multiple lines
* gJ (with the cursor on the first line of a block) to join a block into a single-line statement.
