# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?


- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

  The game loaded successfully, but several parts of its behavior were incorrect. A guess above the secret told me to go higher, while a guess below it told me to go lower. Invalid text also consumed an attempt because the counter increased before the input was validated. I also found that the New Game button did not fully reset the status, score, and history after a game ended.


**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
| Secret was `[50]`; entered `[60]`, which was above the secret | The game should say “Too High” and tell me to guess lower | The outcome was “Too High,” but the displayed hint said “📈 Go HIGHER!” | None | `app.py`, `check_guess()`, around lines 32–40 |
| Entered `hello` and clicked “Submit Guess” | The game should reject the input without using an attempt | The game displayed “That is not a number,” but the attempts counter still increased | `That is not a number.` | `app.py`, submit block around lines 147–154 |
| Finished the game and clicked “New Game” | A new game should begin with playing status, a new score, empty history, and the correct difficulty range | The secret and attempt count changed, but the game still said I had already won or that the game was over; the old score and history also remained | None | `app.py`, `new_game` block around lines 134–145 |

---

## 2. How did you use AI as a teammate?

I used Claude as my coding assistant to help plan tests and Git commands. Claude correctly suggested moving `check_guess` and `parse_guess` from `app.py` into `logic_utils.py`, updating the imports, and correcting the reversed hint messages. I verified that suggestion by reviewing the diff, running pytest, and confirming that guesses above and below the secret produced the correct outcomes and directions. Another AI suggestion was to create a `GameState` class and move all Streamlit session-state management into it, but I did not accept it because it was over-engineered for two focused bugs and would have changed more code than necessary. Instead, I kept the existing structure and moved the attempt increment after input validation, which I verified by entering invalid input and confirming that it did not use an attempt.

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

---

## 3. Debugging and testing your fixes

I considered each bug fixed only after checking both the automated tests and the behavior of the live Streamlit game. I ran `python -m pytest -v`, and all five tests passed, including regression tests that checked that a high guess says “LOWER” and a low guess says “HIGHER.” I also ran the app manually and confirmed that invalid text displayed an error without consuming an attempt. AI helped me design the regression tests, but I reviewed the assertions and test results myself instead of assuming that the generated tests were correct.

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

Streamlit reruns the Python script from top to bottom whenever the user interacts with a widget. Regular local variables can be recreated during a rerun, so they are not reliable for remembering an ongoing game. `st.session_state` stores values such as the secret number, attempt count, score, and status between reruns. State values should be initialized only when their keys do not already exist and deliberately reset when a new game begins.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

One habit I want to reuse is reproducing a bug with a specific input before attempting to change the code. I also want to keep writing small regression tests so that each repair has clear evidence behind it. Next time I work with AI, I will give it one focused problem at a time and review every changed file before accepting the result. I will continue using Git commits to create checkpoints after tested changes. This project taught me that AI-generated code can be useful, but it still requires human judgment, testing, and verification.