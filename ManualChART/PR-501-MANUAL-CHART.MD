## PR Review Requirements & Suggestions

### UI / Controls
[ ] Update the controls button to be a **settings icon only** with a **tooltip** (similar to the map settings control).
  - [ ] This will work better in dynamic views and keep the UI cleaner.
  - [ ] Replace the current button-style control with an icon-only version.

- [ ] **Remove the manual mode toggle**.
  - [ ] If a user opens settings and changes values, it’s implied they are making manual changes.
  - [ ] The current behaviour was confusing and prevented expected edits.

---

### Layout & Spacing
 - [ ] **Margin Size**
  - [ ] Medium cannot be selected reliably (sometimes only Small and Large work).
  - [ ] Selection behaviour appears buggy.
  - [ ] Increase the visual difference between margin sizes so changes are obvious.

 - [ ] Move **Legend settings above Margin Size** in the Layout section.
  - [ ] Legend settings are more likely to be altered.

---

### Axis & Ticks
 - [ ] **Changing tick values causes a 500 error**
  - [ ] Fix the error and add tests.
  - [ ] Test edge cases such as:
    - [ ] Comma-separated values (with and without spaces)
    - [ ] Invalid input
    - [ ] Input sanitisation (ensure users cannot input code or unsafe values)

 - [ ] **Turning off “Show X Axis”**
  - [ ] Currently removes only the label, not the ticks.
  - [ ] Tick labels overlap and become unreadable.

---

### Time Interval
 - [ ] **Time interval input does not appear editable**.
  - [ ] Text input is prone to error.
  - [ ] Replace with **two dropdowns**:
    - [ ] Value (e.g. 0–60 for minutes, 0–24 for hours)
    - [ ] Unit (minutes, hours, etc.)
  - [ ] This would allow removal of the current text input.

 - [ ] Note:
  - [ ] Time series is usually on the X axis, but not always.
  - [ ] Time controls may not always make sense.
  - [ ] Worth discussing with the team how to handle this better.
  - [ ] Start with the dropdown approach and iterate later.

---

### Style
 - [ ] **Line width**
  - [ ] Good idea, but dragging is clunky.
  - [ ] For charts with two lines (on different axes), ideally support per-line width.
  - [ ] If complex, treat as a follow-up (nice-to-have).

 - [ ] **Enable fill area**
  - [ ] Currently appears to have no effect.

 - [ ] **Axis grid line controls**
  - [ ] “Show grid lines” appears to control the wrong axis:
    - [ ] Y-axis toggle affects the X axis.
    - [ ] X-axis toggle does not work.
  - [ ] Rename to:
    - [ ] **Show Axis Line**, or
    - [ ] **Show Axis Grid Line**
  - [ ] Current naming is unclear, especially with grid options in the Style section.

 - [ ] **Border colour**
  - [ ] Purpose is unclear.
  - [ ] Does not appear to change any visible colour.

 - [ ] **Missing control**
  - [ ] Add the ability to **change the line colour**.
  - [ ] Keep colour options simple for now.

---

### Quality & Testing
 - [ ] Overall approach is good, but there are multiple bugs.
 - [ ] Please:
  - [ ] Carefully test all controls
  - [ ] Add tests to catch regressions and edge cases