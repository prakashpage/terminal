# COOKED_READ_DATA, aka conhost's readline implementation

## Test instructions

All of the following ✅ marks must be fulfilled during manual testing:
* ASCII input
* Chinese input (中文維基百科) ❔
  * Resizing the window properly wraps/unwraps wide glyphs ❌
    Broken due to `TextBuffer::Reflow` bugs
* Surrogate pair input (🙂) ❔
  * Resizing the window properly wraps/unwraps surrogate pairs ❌
    Broken due to `TextBuffer::Reflow` bugs
* In cmd.exe
  * Create 2 file: "a😊b.txt" and "a😟b.txt"
  * Press tab: Autocomplete to "a😊b.txt" ✅
  * Navigate the cursor right past the "a"
  * Press tab twice: Autocomplete to "a😟b.txt" ✅
* Backspace deletes preceding glyphs ✅
* Ctrl+Backspace deletes preceding words ✅
* Escape clears input ✅
* Home navigates to start ✅
* Ctrl+Home deletes text between cursor and start ✅
* End navigates to end ✅
* Ctrl+End deletes text between cursor and end ✅
* Left navigates over previous code points ✅
* Ctrl+Left navigates to previous word-starts ✅
* Right and F1 navigate over next code points ✅
  * Pressing right at the end of input copies characters
    from the previous command ✅
* Ctrl+Right navigates to next word-ends ✅
* Insert toggles overwrite mode ✅
* Delete deletes next code point ✅
* Up and F5 cycle through history ✅
  * Doesn't crash with no history ✅
  * Stops at first entry ✅
* Down cycles through history ✅
  * Doesn't crash with no history ✅
  * Stops at last entry ✅
* PageUp retrieves the oldest command ✅
* PageDown retrieves the newest command ✅
* F2 starts "copy to char" prompt ✅
  * Escape dismisses prompt ✅
  * Typing a character copies text from the previous command up
    until that character into the current buffer (acts identical
    to F3, but with automatic character search) ✅
* F3 copies the previous command into the current buffer,
  starting at the current cursor position,
  for as many characters as possible ✅
  * Doesn't erase trailing text if the current buffer
    is longer than the previous command ✅
  * Puts the cursor at the end of the copied text ✅
* F4 starts "copy from char" prompt ✅
  * Escape dismisses prompt ✅
  * Erases text between the current cursor position and the
    first instance of a given char (but not including it) ✅
* F6 inserts Ctrl+Z ✅
* F7 without modifiers starts "command list" prompt ✅
  * Escape dismisses prompt ✅
  * Minimum size of 40x10 characters ✅
  * Width expands to fit the widest history command ✅
  * Height expands up to 20 rows with longer histories ✅
  * F9 starts "command number" prompt ✅
  * Left/Right paste replace the buffer with the given command ✅
    * And put cursor at the end of the buffer ✅
  * Up/Down navigate selection through history ✅
    * Stops at start/end with <10 entries ✅
    * Stops at start/end with >20 entries ✅
    * Wide text rendering during pagination with >20 entries ✅
  * Shift+Up/Down moves history items around ✅
  * Home navigates to first entry ✅
  * End navigates to last entry ✅
  * PageUp navigates by 20 items at a time or to first ✅
  * PageDown navigates by 20 items at a time or to last ✅
* Alt+F7 clears command history ✅
* F8 cycles through commands that start with the same text as
  the current buffer up until the current cursor position ✅
  * Doesn't crash with no history ✅
* F9 starts "command number" prompt ✅
  * Escape dismisses prompt ✅
  * Ignores non-ASCII-decimal characters ✅
  * Allows entering between 1 and 5 digits ✅
  * Pressing Enter fetches the given command from the history ✅
* Alt+F10 clears doskey aliases ✅
* In cmd.exe, with an empty prompt in an empty directory:
  Pressing tab produces an audible bing and prints no text ✅
* When Narrator is enabled, in cmd.exe:
  * Typing individual characters announces only
    exactly each character that is being typed ✅
  * Backspacing at the end of a prompt announces
    only exactly each deleted character ✅
