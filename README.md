# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

The purpose of this project was to debug an AI-generated number-guessing game and verify the repairs with automated and manual tests. The original game displayed reversed higher and lower instructions, counted invalid input as an attempt, and did not completely reset its state when starting a new game. I moved `check_guess` and `parse_guess` from `app.py` into `logic_utils.py`, corrected the hint directions, and changed the submit logic so invalid input does not consume an attempt. I reviewed the AI-generated changes and added pytest regression tests to confirm the repaired comparison logic.

## 📸 Demo Walkthrough

1. The game starts on Normal difficulty and selects a secret number, such as 50.
2. The user enters 40, and the game returns “Too Low” with the instruction “Go HIGHER!”
3. The user enters 60, and the game returns “Too High” with the instruction “Go LOWER!”
4. The user enters invalid text such as `hello`, and the game displays an error without consuming an attempt.
5. The user enters 50, and the game recognizes the correct guess.
6. The game displays the winning message and final score.

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```text
$ python -m pytest -v
collected 5 items

tests/test_game_logic.py::test_winning_guess PASSED
tests/test_game_logic.py::test_guess_too_high PASSED
tests/test_game_logic.py::test_guess_too_low PASSED
tests/test_game_logic.py::test_too_high_message_says_go_lower PASSED
tests/test_game_logic.py::test_too_low_message_says_go_higher PASSED

5 passed in 0.01s


## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
