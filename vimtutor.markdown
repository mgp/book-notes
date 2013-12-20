### Lesson 2.3: On Operations and Motions
* `w` is until the start of the next word, excluding its first character.
* `e` is until the end of the current word, including the last character.

### Lesson 2.6: Operating on Lines
* In the command `dd`, the second `d` isn't a motion. It's just a shortcut by the vim designers.

### Lesson 2.7: The Undo Command
* Typing a capital `U` will undo all changes on a line.
* Typing `Ctrl`+`R` will redo the commands, or undo the undos.

### Lesson 3.3: The Change Operator
* To change until the end of a word, type `ce`.

### Lesson 3.4: More Changes Using `c`
* The change operator accepts a number and a motion like `d`.

### Lesson 4.1: Cursor Location and File Status
* Typing `Ctrl`+`g` will display the filename and the position in the file.
* Typing `gg` will move to the start of a file, and typing `G` moves to the bottom.

### Lesson 4.2: The Search Command
* Typing `Ctrl`+`o` will jump to an older location. Typing `Ctrl`+`i` will jump to a newer location.

### Lesson 4.4: The Substitute Command
* To substitute only on the current line, type `:s/old/new`.
* To substitute in a range of lines, type `:#,#s/old/new/g`.
* To find each occurrence and prompt for substitution, type `:%s/old/new/gc`.

### Lesson 5.3: Selecting Text to Write
* To save part of a file, select it in visual mode before executing `:w`.

### Lesson 5.4: Retrieving and Merging Files
* To insert the contents of a file, type `:r FILENAME`.

### Lesson 6.3: Another Way to Replace
* Type `R` to replace more than one character.

### Lesson 6.4: Copy and Paste Text
* You can also use `y` as an operator, e.g. `yw` yanks one word.

### Lesson 6.5: Set Option
* If you want to ignore case for just one search command, use `/[term]\c.

### Lesson 7.1: Getting Help
* Type `:help` to get help. Add an argument to get help on a particular subject.

### Lesson 7.3: Completion
* Tab completion works for commands, as well as filenames following `:e` or `:w`.

