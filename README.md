��# QuizAppGold

An accessible, state-driven, vanilla JavaScript quiz application demonstrating production-grade front-end patterns (state isolation, pure rendering, event delegation, progressive enhancement, persistence, and a11y support).

---

## Features

- Accessible UI (ARIA roles, live region, focus management)
- Keyboard navigation (ArrowUp / ArrowDown / Enter)
- Single source of truth state object
- Pure re-render per question (no fragile incremental DOM patching)
- Option selection with `aria-pressed`
- Configurable question shuffling
- LocalStorage session persistence + resume prompt
- Progress bar with `aria-valuenow`
- Secure HTML escaping for dynamic content
- Results summary with per-question correctness
- Restart flow without page reload
- Easily extensible architecture (timers, categories, analytics)

---

## Tech Stack

- HTML5 (semantic regions)
- CSS3 (design tokens, utility classes)
- Vanilla ES6+ JavaScript (no dependencies)
- LocalStorage (persistence)

---

## Architecture Overview

| Layer          | Responsibility                                     |
|----------------|----------------------------------------------------|
| CONFIG         | Runtime feature switches (shuffle, persistence)    |
| Data           | `baseQuestions` immutable source                   |
| Runtime Copy   | `questions` (cloned & optionally shuffled)         |
| State          | `state` object (progress, score, answers)          |
| Render         | `renderQuestion()`, `showResults()`                |
| Control Flow   | `handleSelect()`, `goNext()`, `restartQuiz()`      |
| Persistence    | `persistState()`, `tryResume()`                    |
| Accessibility  | Live region, roles, `aria-pressed`, focus restore  |
| Input Handling | Click delegation + global key listener             |

---

## State Shape

```js
state = {
  currentIndex: Number,
  selected: String | null,
  score: Number,
  answers: [
    { questionIndex, selected, correctAnswer, isCorrect }
  ],
  startedAt: Number
}
```

---

## Data Flow

1. Startup → (optional) resume → `renderQuestion()`
2. User selects option → `handleSelect()`
3. User clicks Next / Finish → `goNext()`
4. Answer recorded → score updated → persisted → next or results
5. Results → restart → `restartQuiz()` → fresh session

---

## Accessibility

- Native buttons for options
- `aria-pressed` reflects selection
- `role="group"` wraps question block
- `aria-live` announcements for selection & completion
- Focus placed on first option each render
- Full keyboard navigation

### Keyboard Shortcuts

| Key        | Action                          |
|------------|----------------------------------|
| ArrowDown  | Next option (wrap)              |
| ArrowUp    | Previous option (wrap)          |
| Enter      | Advance if selection made       |

---

## Persistence

- Session saved after each submitted answer
- Resume prompt on reload
- Cleared automatically at completion or restart

---

## Progress

- Visual bar width indicates advancement
- `aria-valuenow` exposes percent to assistive tech
- Text fallback: “Question X of Y”

---

## Security Considerations

- All dynamic text escaped with `escapeHTML()`
- Safe for future user-submitted question sources (with validation)

---

## Project Structure

```
practicejs/
  quiz.html
  quiz.css
  quiz.js
  README.md
```

---

## Customization

| Goal                   | Change                                   |
|------------------------|-------------------------------------------|
| Add questions          | Append to `baseQuestions`                 |
| Disable shuffle        | `CONFIG.enableShuffle = false`            |
| Disable announcements  | `CONFIG.announceSelections = false`       |
| Change persist key     | Edit `CONFIG.persistKey`                  |
| Add categories         | Add `category` to each question           |

---

## Extensibility Ideas

- Timers (per-question or global)
- Category filtering + breakdown
- Adaptive difficulty
- Review / revisit mode
- Analytics (avg time, streaks)
- Theming (light/dark via CSS vars)
- Export results (JSON / CSV)

---

## Example Question

```js
{
  question: "Who wrote 'Hamlet'?",
  options: ["Charles Dickens", "William Shakespeare", "Mark Twain", "Jane Austen"],
  answer: "William Shakespeare"
}
```

---

## Core Functions

| Function          | Purpose                                      |
|-------------------|----------------------------------------------|
| renderQuestion()  | Render current question                      |
| handleSelect()    | Apply selection & enable Next                |
| goNext()          | Record answer & advance / finish             |
| showResults()     | Display summary + restart action             |
| restartQuiz()     | Reset state and begin again                  |
| tryResume()       | Restore saved session if valid               |
| persistState()    | Save session to LocalStorage                 |
| escapeHTML()      | Sanitize inserted text                       |

---

## Manual Test Scenarios

| Scenario                          | Expected Outcome                      |
|----------------------------------|----------------------------------------|
| No selection + Next              | Button disabled (no action)           |
| Finish on last question          | Results displayed                     |
| Restart after finish             | First question shown, score reset     |
| Refresh mid-quiz → Resume Yes    | Continues where left off              |
| Refresh mid-quiz → Resume No     | Starts fresh                          |
| Arrow navigation                 | Selection + focus follow              |

---

## License

MIT (add LICENSE file if required).

---

## Status

Stable baseline. Ready for timers, review mode, theming, analytics.
