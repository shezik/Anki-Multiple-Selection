# Anki-Multiple-Selection

Sorry for the AIGC README. It worked out rather well.

Actual code is not AI slop though.

## Introduction

**Anki-Multiple-Selection** is a fork of [ale18V/Anki-Multiple-Choice](https://github.com/ale18V/Anki-Multiple-Choice) that adds support for **multiple‑selection questions** with an arbitrary number of choices.  
It provides an interactive way to answer multiple‑choice cards, randomizes the order of choices to prevent position‑based memorization, and gives visual feedback on the back side.

## Features

- Unlimited number of answer choices.
- Supports both **single‑answer** (auto‑flip on selection) and **multiple‑answer** (flip manually) modes.
- Choices are shuffled by default (can be disabled).
- Color‑coded feedback on the back side:
  - **Green** – correctly chosen.
  - **Orange** – correct answer that was missed.
  - **Red** – incorrect choice that was selected.
- **Hotkey support** – press F6–F10 to select choices 1-5 on desktop, or use User Actions on AnkiDroid.
- Works both on desktop and mobile.

## How to set up

### 1. Create a new note type

In the **Manage Note Types** window (Ctrl+Shift+N), add a **Basic** note type, and click **Fields**.  
Add the following five fields:

| Field Name | Required | Description |
|------------|----------|-------------|
| `Front`    | Yes      | The question or prompt. |
| `Back`     | No       | The answer explanation. |
| `Choices`  | Yes      | The list of answer choices. See [Choices format](#choices-format) below. |
| `Answer`   | Yes      | Indicates which choice(s) are correct. See [Answer format](#answer-format) below. |
| `Shuffle?` | No       | If set to `n` or `N`, the choices are **not** shuffled; otherwise shuffled by default. |

### 2. Copy the card templates

Still in the note type window, click **Cards**. Replace the contents of the three tabs with the code from this repository:

- **Front Template** → copy `Front Template.html`
- **Back Template**  → copy `Back Template.html`
- **Styling**        → copy `Styling.css`

### 3. Create notes

When you add a new note, fill in the fields as follows.

#### Choices format

Each choice must be separated by either:
- a **single line break** (if no choice contains internal line breaks), or  
- a **blank line** (two consecutive line breaks) if any choice itself contains line breaks.

The parser checks the entire `Choices` field:
- If there is **no blank line**, each line becomes a separate choice (single `<br>` is the separator).
- If **at least one blank line** is present, **all** choices are split by blank lines.  
  Ordinary single line breaks inside a choice are then preserved as part of that choice.

| Example Input | HTML Equivalent | Resulting Choices |
|---------------|-----------------|-------------------|
| <pre>Paris<br>London<br>Berlin<br>Rome</pre> | `Paris<br>London<br>Berlin<br>Rome` | 1. Paris<br>2. London<br>3. Berlin<br>4. Rome |
| <pre>Paris<br>London<br><br>Berlin<br>Rome</pre> | `Paris<br>London<br><br>Berlin<br>Rome` | 1. Paris<br>London<br>2. Berlin<br>Rome |

#### Answer format

The `Answer` field tells the template which choices are correct. It accepts two styles:

- **Letters** (case‑insensitive): `A`, `B`, `C`, … corresponding to the order of choices as they appear in the `Choices` field.  
  For multiple correct answers, simply write them consecutively, e.g. `ACD` (choices 1, 3, and 4 are correct).

- **Numbers** (starting at 1): Same as above. For numbers above 10, you must separate each one with `<br>`.  

| Example `Answer` content | Meaning |
|--------------------------|---------|
| `B`                      | Only the second choice is correct.  |
| `ACD`                    | Choices 1, 3, and 4 are correct.    |
| `246`                    | Choices 2, 4, and 6 are correct.    |
| `12<br>13<br>14`         | Choices 12, 13, and 14 are correct. |

**Important:**  
- Numbers correspond to the **original** order of choices, not the shuffled order displayed on the card.  
- For a single correct answer, the card will automatically flip as soon as you select it.  
- For multiple correct answers, you must manually click **Show Answer** (or use the Anki shortcut) to see the back side.

#### Shuffle? field

Leave this field **empty** to shuffle the choices every time you review the card.  
If you type `n` (or `N`) in this field, the choices will appear in the exact order you entered them.

## Using the template

- **Desktop**: click on a choice, or use the hotkeys **F6**–**F10** for the first five choices.
- **Mobile**: tap the choice. For AnkiDroid users, User Actions are also available.

On the front side, selected choices are highlighted with a light blue border.

On the back side, you will see:
- **Green** – choices you correctly selected.
- **Orange** – correct choices you missed (only shown in multiple‑answer mode).
- **Red** – incorrect choices you selected.

If there is more than one correct answer, a small label **“Multiple Selection”** appears above the choices. Flip the card manually to confirm your selection.

## Debugging

Both templates include a hidden paragraph:

```html
<p id="multiple-choice-debug"></p>
```

If something goes wrong (e.g., persistence unavailable, invalid answer format), an error message will appear there. You can keep it – it stays invisible unless an error occurs.

## Contributing

Feel free to improve the templates or share your own designs. Pull requests are welcome!  
If you create a beautiful note type using this library, share it in the Discussions section so others can benefit.

## License

This project is licensed under the **GNU General Public License v3.0**. See the [LICENSE](LICENSE) file for details.