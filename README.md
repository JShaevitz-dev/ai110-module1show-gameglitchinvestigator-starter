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

- [ ] Describe the game's purpose.
- The game is is a simple program where a random number is chosen and the player guesses the number with the program hinting higher or lower based on the guess.
- [ ] Detail which bugs you found.
- Type-mismatch hint bug — On even attempts the secret was silently converted to a string while the guess stayed an int, forcing a TypeError fallback that compared them lexicographically and gave backwards "Too High"/"Too   Low" hints.
- Off-by-one attempt counter — The initial attempts value (1) didn't match the "New Game" reset value (0), making the "Attempts left" count inconsistent depending on how the session started.
- Hardcoded range text — The main instruction banner always said "between 1 and 100" regardless of difficulty, even though Easy and Hard use different ranges.
- "New Game" ignored difficulty — The New Game button regenerated the secret with a hardcoded 1–100 range instead of the selected difficulty's actual range.
- "New Game" didn't reset score/status/history — Clicking New Game reset the secret and attempts but left the previous game's score, win/loss status, and guess history in place.
- Difficulty switch didn't regenerate the secret — Changing the difficulty dropdown mid-session didn't reroll the secret, so it could remain outside the newly selected range and make the game unwinnable.
- A mismatch between the guess counter's logic and what was being displayed due to uncoordinated indexing.
- Numerous issues with starting a new game.
- Hardcoded difficulty that made the setting moot.
- [ ] Explain what fixes you applied.
- Type-mismatch hint bug — Removed the parity-based str(secret) conversion so check_guess always compares two ints, eliminating the string-comparison fallback that flipped hint directions.
- Off-by-one attempt counter — Changed the initial attempts value from 1 to 0 so it matches the "New Game" reset and the attempts-left math is consistent from the start.
- Hardcoded range text — Replaced the literal "1 and 100" in the instruction banner with the actual low/high variables for the selected difficulty.
- "New Game" ignored difficulty — Replaced the hardcoded random.randint(1, 100) in the New Game button with random.randint(low, high) so the new secret respects the current difficulty.
- "New Game" didn't reset score/status/history — Added resets for score, status, and history to the New Game button so no leftover state carries into the next round.
Difficulty switch didn't regenerate the secret — Added a secret_difficulty tracker that detects a difficulty change and rerolls the secret (plus resets attempts/score/status/history) to match the new range.

## 📸 Demo Walkthrough

Describe your fixed game in numbered steps so a reader can follow along without watching a video:

1. <!-- Describe this step -->
2. <!-- Describe this step -->
3. <!-- Describe this step -->
4. <!-- Describe this step -->
5. <!-- Add more steps as needed -->

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
# Paste your pytest output here, e.g.:
# pytest tests/
# ========================= X passed in 0.XXs =========================
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
