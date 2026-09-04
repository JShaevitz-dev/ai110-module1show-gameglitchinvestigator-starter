# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
|25 | Lower | Higher | None |
| 24| Win | Higher | None |
| 25 | Lower | Higher | None |

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Claude
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- It correctly identified the logic error
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
- It did not make any incorrect suggestions

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
Testing
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
 - Ran three manual tests and all behaved as normal
- Did AI help you design or understand any tests? How?
It helped me understand the logic in the error while running me through the logic.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

- Streamlit reruns the code every time the page refreshes
---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
