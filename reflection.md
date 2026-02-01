# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- The hints to guess the correct number are incorrect
-The New Game button doesnt work
- Confusing final score calculation
- On all difficulties, sometimes I started with the correct number of attempts and others I started with one less than I was supposed to have. 
- Hard Mode says that the number is between 1-50, but the answer was over

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
* Cursor (for code editing and suggestions)
* ChatGPT (for understanding Streamlit concepts)
* Copilot (for code completion)
- Give one example of an AI suggestion you accepted and why.
* Accepted using st.session_state to persist the secret number across reruns. This fixed the issue where the secret changed on every button click.
- Give one example of an AI suggestion you changed or rejected and why.
* Rejected suggestions that didn’t account for difficulty ranges. For example, initial fixes used hardcoded random.randint(1, 100) instead of using low and high from the difficulty setting, which broke Hard mode (1–50).

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
* Manual testing: played the game, checked hints, verified attempts counting, and confirmed the New Game button reset everything.
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
  * Ran test_guess_too_high(): check_guess(60, 50) should return "Too High". This confirmed the hint logic works and that the backwards hint bug was fixed.
- Did AI help you design or understand any tests? How?
* AI explained pytest syntax and how to structure test functions. It also suggested edge cases to test, like comparing integers and strings, which revealed the bug where the secret was converted to a string on even attempts.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
* The original code was generating a new secret number on every rerun because it wasn't stored in session state. Each time you clicked a button, Streamlit reran the entire script from top to bottom, and without st.session_state, the random.randint() call would execute again, creating a brand new number each time. 

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
* Streamlit reruns are like refreshing a webpage. Every time you click a button or change an input, Streamlit runs your entire Python script from the beginning. Without session state, all your variables get reset to their initial values. st.session_state is like a special memory box that survives these reruns. Anything you put in there stays there until you change or delete it. It's the difference between writing on a whiteboard that gets erased (regular variables) versus writing in a notebook that you keep (session state).

- What change did you make that finally gave the game a stable secret number?
* I stored the secret number in st.session_state.secret and used a check if "secret" not in st.session_state: to only generate it once when it doesn't exist. This ensures the secret is created on the first run and then persists across all subsequent reruns. When the New Game button is clicked, it explicitly resets st.session_state.secret to a new random number, giving us control over when it changes.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
* Using the Developer Debug Info tab to inspect session state values in real time was incredibly helpful. Being able to see exactly what values were stored and how they changed with each interaction made debugging much easier. I want to make this a habit always include debug information when working with state management, even if it's just temporary during development.

- What is one thing you would do differently next time you work with AI on a coding task?
* I would test each fix individually before moving to the next bug. I fixed multiple issues at once, which made it harder to isolate which change caused which behavior. Next time, I'll fix one bug, test it thoroughly, commit it, then move to the next one. This makes debugging easier and helps me understand the impact of each change.

- In one or two sentences, describe how this project changed the way you think about AI generated code.
* This project showed me that AI-generated code can look correct and run without errors, but still contain subtle logical bugs (like backwards hints or type conversion issues). I learned to always test thoroughly and not assume code works just because it executes. The code might be syntactically correct but logically flawed, requiring careful manual testing and verification.