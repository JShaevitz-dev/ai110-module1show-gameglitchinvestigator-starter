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

1. Run program with streamlight
2. Choose difficulty
3. Enter guesses until win or lose condition

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
============================================= test session starts ==============================================
platform win32 -- Python 3.10.11, pytest-9.1.1, pluggy-1.6.0 -- C:\Users\Coldfur\AppData\Local\Programs\Python\Python310\python.exe
cachedir: .pytest_cache
rootdir: C:\Users\Coldfur\Documents\Code\ai110-module1show-gameglitchinvestigator-starter-main
plugins: anyio-4.13.0, hydra-core-1.3.2
collected 41 items                                                                                              

test_app_behavior.py::TestAttemptCounter::test_fresh_session_starts_at_zero_attempts PASSED               [  2%]
test_app_behavior.py::TestAttemptCounter::test_attempts_increments_by_one_per_submit PASSED               [  4%]
test_app_behavior.py::TestAttemptCounter::test_fresh_session_and_new_game_agree_on_starting_attempts PASSED [  7%]
test_app_behavior.py::TestNewGameDifficultyRange::test_new_game_secret_respects_easy_range PASSED         [  9%]
test_app_behavior.py::TestNewGameDifficultyRange::test_new_game_secret_respects_hard_range PASSED         [ 12%]
test_app_behavior.py::TestNewGameResetsState::test_new_game_resets_score PASSED                           [ 14%]
test_app_behavior.py::TestNewGameResetsState::test_new_game_resets_history PASSED                         [ 17%]
test_app_behavior.py::TestNewGameResetsState::test_new_game_resets_status_after_a_win PASSED              [ 19%]
test_app_behavior.py::TestDifficultySwitchRegeneratesSecret::test_switching_to_easy_puts_secret_in_range PASSED [ 21%]
test_app_behavior.py::TestDifficultySwitchRegeneratesSecret::test_switching_difficulty_resets_progress PASSED [ 24%]
test_app_behavior.py::TestDifficultySwitchRegeneratesSecret::test_switching_back_and_forth_keeps_secret_in_current_range PASSED [ 26%]
test_app_behavior.py::TestGameLockAfterWin::test_submitting_after_a_win_does_not_increment_attempts_further PASSED [ 29%]
test_game_logic.py::TestGetRangeForDifficulty::test_returns_expected_range[Easy-expected0] PASSED         [ 31%]
test_game_logic.py::TestGetRangeForDifficulty::test_returns_expected_range[Normal-expected1] PASSED       [ 34%]
test_game_logic.py::TestGetRangeForDifficulty::test_returns_expected_range[Hard-expected2] PASSED         [ 36%]
test_game_logic.py::TestGetRangeForDifficulty::test_returns_expected_range[Nonsense-expected3] PASSED     [ 39%]
test_game_logic.py::TestGetRangeForDifficulty::test_returns_expected_range[-expected4] PASSED             [ 41%]
test_game_logic.py::TestParseGuess::test_valid_integer_string PASSED                                      [ 43%]
test_game_logic.py::TestParseGuess::test_valid_float_string_truncates PASSED                              [ 46%]
test_game_logic.py::TestParseGuess::test_negative_number PASSED                                           [ 48%]
test_game_logic.py::TestParseGuess::test_empty_string_is_rejected PASSED                                  [ 51%]
test_game_logic.py::TestParseGuess::test_none_is_rejected PASSED                                          [ 53%]
test_game_logic.py::TestParseGuess::test_non_numeric_string_is_rejected PASSED                            [ 56%]
test_game_logic.py::TestParseGuess::test_whitespace_only_is_rejected PASSED                               [ 58%]
test_game_logic.py::TestCheckGuess::test_exact_match_is_a_win PASSED                                      [ 60%]
test_game_logic.py::TestCheckGuess::test_guess_above_secret_says_go_lower PASSED                          [ 63%]
test_game_logic.py::TestCheckGuess::test_guess_below_secret_says_go_higher PASSED                         [ 65%]
test_game_logic.py::TestCheckGuess::test_direction_is_numeric_not_lexicographic[99-100-Too Low] PASSED    [ 68%]
test_game_logic.py::TestCheckGuess::test_direction_is_numeric_not_lexicographic[9-10-Too Low] PASSED      [ 70%]
test_game_logic.py::TestCheckGuess::test_direction_is_numeric_not_lexicographic[100-99-Too High] PASSED   [ 73%]
test_game_logic.py::TestCheckGuess::test_direction_is_numeric_not_lexicographic[10-9-Too High] PASSED     [ 75%]
test_game_logic.py::TestCheckGuess::test_direction_is_numeric_not_lexicographic[2-100-Too Low] PASSED     [ 78%]
test_game_logic.py::TestCheckGuess::test_result_is_independent_of_attempt_parity PASSED                   [ 80%]
test_game_logic.py::TestUpdateScore::test_win_awards_decreasing_points_for_later_attempts PASSED          [ 82%]
test_game_logic.py::TestUpdateScore::test_win_score_never_drops_below_floor_of_ten PASSED                 [ 85%]
test_game_logic.py::TestUpdateScore::test_too_high_on_even_attempt_awards_points PASSED                   [ 87%]
test_game_logic.py::TestUpdateScore::test_too_high_on_odd_attempt_deducts_points PASSED                   [ 90%]
test_game_logic.py::TestUpdateScore::test_too_low_always_deducts_points PASSED                            [ 92%]
test_game_logic.py::TestUpdateScore::test_unknown_outcome_leaves_score_unchanged PASSED                   [ 95%]
test_game_logic.py::TestFullRoundIntegration::test_correct_guess_end_to_end PASSED                        [ 97%]
test_game_logic.py::TestFullRoundIntegration::test_wrong_guess_end_to_end_gives_correct_direction PASSED  [100%]

============================================== 41 passed in 4.38s ==============================================
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
