# Manual Testing Guide for Insert Mode Modifier Keys

This guide helps you test the new feature that allows modifier key shortcuts to work in insert mode.

## Setup

1. Install the extension from source (see CONTRIBUTING.md)
2. Open any web page with an input field (e.g., https://www.google.com)

## Test Cases

### Test 1: Alt+E to Visit Previous Tab (New Default Mapping)

1. Open at least 3 tabs
2. Switch between tabs to create a tab history
3. Focus on an input field (e.g., the search box)
4. Press `Alt+E`
5. **Expected**: Should switch to the previously visited tab without needing to press Escape first

### Test 2: Alt+P to Toggle Pin (Existing Mapping)

1. Open a tab
2. Focus on an input field
3. Press `Alt+P`
4. **Expected**: Should pin/unpin the current tab without needing to press Escape first

### Test 3: Alt+M to Toggle Mute (Existing Mapping)

1. Open a tab with audio or video
2. Focus on an input field
3. Press `Alt+M`
4. **Expected**: Should mute/unmute the current tab without needing to press Escape first

### Test 4: Regular Keys Still Work Normally in Insert Mode

1. Focus on an input field
2. Type normal letters (e.g., "hello")
3. **Expected**: Text should be typed normally, no Vimium commands should be triggered

### Test 5: Non-Modifier Keys Don't Work in Insert Mode

1. Focus on an input field
2. Press `j` (which normally scrolls down)
3. **Expected**: The letter "j" should be typed, not scroll the page

### Test 6: Custom Mapping with Modifiers

1. Go to Vimium options
2. Add a custom mapping: `map <a-n> nextTab`
3. Save
4. Focus on an input field
5. Press `Alt+N`
6. **Expected**: Should switch to the next tab

## What This Feature Does

- Background commands (commands handled by the background script) that use modifier keys (Alt, Ctrl, Meta)
  are now available in insert mode
- This allows you to perform tab operations and other background commands without leaving insert mode
- Regular keys (without modifiers) still work normally in insert mode to avoid interfering with typing

## Implementation Details

The feature works by:
1. Filtering background commands that have modifier keys when key mappings are loaded
2. Storing these commands in session storage as `insertModeCommands`
3. Checking for these commands in insert mode's key handler before passing keys to the page
4. Sending a message to the background script to execute the command when matched
