# 5-Stage Project Plan: Imperial HBR Prep + Employability
### Basile — Software-first, Elegoo UNO R3 Super Starter Kit x2, Windows

**How to use this doc:** Work top to bottom. Don't skip Stage 0 even though it's boring — everything downstream depends on it. Check boxes as you go. Each stage lists *options*, not a single mandated path — pick one, don't try to do all options in a stage.

---

## STAGE 0 — Git & GitHub Basics
**Time: 2-4 hours. This is infrastructure, not learning. Get it working, don't over-study it.**

Brutally honest: this stage is dull and that's fine. You need exactly four commands and one habit (commit often, with a real message). Do not read a full Git course — that's a trap that eats 10 hours for something you need 2 hours of.

### What to actually do
- [x] Create a GitHub account (if you don't have one)
- [x] Install Git for Windows: https://git-scm.com/download/win
- [x] Set your identity once in a terminal:
  ```
  git config --global user.name "Your Name"
  git config --global user.email "you@email.com"
  ```
- [x] Create one repo on GitHub called `learning-log` or similar
- [x] Clone it locally, add a file, and run: `git add .` → `git commit -m "message"` → `git push`
- [ ] Do this once a day minimum once you start Stage 1 — small, frequent commits, not one giant commit at the end

### Options for *how* you learn the mechanics
- **Option A — GitHub's own interactive tutorial**: https://skills.github.com/ (free, ~1-2 hrs, "Introduction to GitHub" module). Most efficient if you want minimum viable competence.
- **Option B — VS Code's built-in Git panel**: Skip the terminal almost entirely and use VS Code's Source Control tab (buttons for stage/commit/push). Good if command line intimidates you; you'll pick up the terminal commands naturally later.
- **Option C — GitHub Desktop app**: https://desktop.github.com/ — GUI-only, zero command line. Fastest to start, but you'll want to learn the actual commands eventually since terminal Git is the industry norm and what employers assume you know.

**Honest recommendation: Option B.** VS Code is what you'll code in anyway, the Source Control panel removes the intimidation factor, and you'll absorb the underlying commands by seeing what VS Code runs. Don't use GitHub Desktop long-term — it's a crutch that doesn't transfer to a professional environment.

### Common failure points on Windows
- Git not added to PATH → reinstall and check "Git from the command line" option
- Line-ending warnings (`LF will be replaced by CRLF`) — ignore these, they're not errors
- Getting stuck in Vim if a commit message editor pops up unexpectedly — type `:wq` and hit enter, or better, always use `git commit -m "message"` to avoid opening an editor at all

---

## STAGE 1 — Python "Blank File" Fluency
**Time: 10-15 hours over 1-2 weeks.**

Your stated gap isn't syntax — it's freezing without a tutorial to follow. Every option below is deliberately *not* a course. You look up syntax as needed (that's normal, professionals do this constantly), but the structure/logic must be yours.

### Setup (do once)
- [ ] Install Python via https://www.python.org/downloads/ (check "Add to PATH" during install)
- [ ] Install VS Code: https://code.visualstudio.com/
- [ ] Install the Python extension inside VS Code
- [ ] Confirm it works: create `test.py`, write `print("hello")`, run it

### Pick 3 of these — no tutorial, no reference project, just the goal and a blank file
1. **Unit converter** (temperature, distance, or currency) — command-line, takes an input, prints an output
2. **To-do list with file persistence** — add/remove/view tasks, saves to a `.txt` or `.json` file, reloads on restart
3. **Number-guessing game** — random number, loop until correct, track attempt count
4. **Simple budget/expense tracker** — log expenses with categories, print a total, save/load from file
5. **Basic dice-roll or card-game simulator** — introduces randomness and simple game loops

For each: if you get stuck on *syntax* (e.g. "how do I read a file in Python"), look it up — that's fine, everyone does this. If you get stuck on *what to build next*, that's the freeze you're fixing — sit with it for 10 minutes before searching, then search for the concept (not the finished code).

- [ ] Push each as its own repo with a short README: what it does, how to run it

### Resources if you need a syntax refresher (reference, not tutorial-following)
- **Python official docs tutorial**: https://docs.python.org/3/tutorial/ — best for looking up a specific thing (e.g. "how do dictionaries work") rather than following start to finish
- **W3Schools Python reference**: https://www.w3schools.com/python/ — fast lookup with runnable examples, good for "what's the syntax for X"

---

## STAGE 2 — Python ↔ Arduino Serial Bridge
**Time: ~10 hours over 1 week. This is the actual pivot point of the whole plan.**

This is where software meets hardware and where C exposure starts. Your Elegoo kit already has what you need: an ultrasonic sensor, a potentiometer, and an SG90 servo motor — all standard in the Super Starter Kit.

### Setup
- [ ] Install Arduino IDE: https://www.arduino.cc/en/software
- [ ] Plug in the UNO R3, select the correct board + COM port in the IDE (Windows: check Device Manager if the port doesn't show up — this is the single most common Windows Arduino headache)
- [ ] Run the built-in `Blink` example first (File → Examples → 01.Basics → Blink) to confirm the board and IDE talk to each other before touching your own code
- [ ] In Python: `pip install pyserial`

### Pick ONE direction to start (do the other after)
**Option A — Arduino reads, Python listens**
- Arduino: read a sensor (ultrasonic distance, or the potentiometer) and `Serial.println()` the value repeatedly
- Python: open the serial port with `pyserial`, read incoming lines, print them — then extend to log to a `.csv` or live-plot with `matplotlib`

**Option B — Python commands, Arduino acts**
- Python: send a value or command over serial (e.g. an angle)
- Arduino: read the incoming serial data, parse it, move the SG90 servo to that angle

**Honest recommendation: start with Option A.** It's lower-friction (no parsing logic needed on the Arduino side yet) and gives you an immediate visible result (numbers streaming into Python) that confirms the whole chain works before you add complexity.

### Resources
- **Arduino's own serial communication tutorial**: https://www.arduino.cc/en/Serial/read (official, accurate, short)
- **pySerial docs**: https://pyserial.readthedocs.io/en/latest/shortintro.html
- **Random Nerd Tutorials — Arduino + Python serial**: https://randomnerdtutorials.com/ — search their site for "Arduino Python serial," well-regarded in the hobbyist/maker space for working, tested code (not aggregator spam)
- **Elegoo's own kit tutorial PDF** — you already have this bundled with your kit (check the CD/download link on Elegoo's site: https://www.elegoo.com/download) — it has lesson-by-lesson code for each sensor/module in the box, useful as a reference for wiring diagrams specifically, not as the source of your Python logic

### Common failure points
- Only one program can hold the serial port open at once — if the Arduino IDE's Serial Monitor is open, Python's `pyserial` will fail to connect. Close one before opening the other.
- Baud rate mismatch between Arduino (`Serial.begin(9600)`) and Python (must match) — a classic silent-failure bug
- Servo jitter/reset on connect — normal, it's the servo re-homing; not a bug worth chasing

---

## STAGE 3 — Flagship Project: Closed-Loop Controller
**Time: 40-50 hours over 3-4 weeks. This is your headline portfolio piece.**

This is where you build something that actually maps onto Robotics 1's real content (kinematics/dynamics/**control**) rather than just "sensor talks to motor." A closed loop means: sense → compute error → act → re-sense, continuously.

### Options — pick one based on what your kit + spare parts allow
**Option A — Servo-based position holder (PID from scratch)**
Use the potentiometer as a stand-in "position sensor" and the SG90 servo as the actuator. Read a target position (could just be another potentiometer as a dial you turn, or a fixed setpoint), read current position, compute error, and correct it with a PID loop you write yourself (not a library) — proportional first, then add integral/derivative once proportional-only works and you can see its limitations (overshoot, steady-state error).

**Option B — Obstacle-avoiding or line-following small robot**
Needs a chassis/wheels, which your two starter kits alone may not include (the *Super Starter Kit* is sensor/board-focused, not a robot-car kit — check whether either of your two kits is actually the *Smart Robot Car Kit* variant, which does include a chassis). If not, this option needs an additional ~£20-30 chassis+wheels kit. Uses the ultrasonic sensor + motors to sense-and-react in real time.

**Option C — Temperature or light-regulated system**
Use the DHT11 (temperature) or a photoresistor plus a fan/LED as actuator, holding a target condition via feedback. Simplest wiring, still a genuine closed loop, less visually impressive than A or B for a portfolio/demo.

**Honest recommendation: Option A**, specifically because it's the one that teaches PID — the actual conceptual link to "Human Neuromechanical Control and Learning" and control-theory content in Robotics 1 — without requiring you to buy anything extra. Option B is more impressive as a demo but adds a hardware-acquisition dependency and more wiring/mechanical complexity that can eat your time budget on things unrelated to the actual learning goal.

### What "done" looks like
- [ ] Working closed-loop demo (video worth recording for your portfolio)
- [ ] GitHub repo with: code, wiring diagram/photo, a README explaining the control approach
- [ ] A written note (even just in the README) on what didn't work first and how you found/fixed it — this failure narrative is what makes the project credible to a reader, not just the fact it works

### Resources
- **PID explained simply (Brett Beauregard's classic write-up)**: http://brettbeauregard.com/blog/2011/04/improving-the-beginners-pid-introduction/ — widely regarded as the best plain-English walkthrough of PID for hobbyist/embedded contexts
- **Arduino PID library** (for reference/comparison only, after you've built your own from scratch — don't start with the library or you skip the learning): https://playground.arduino.cc/Code/PIDLibrary/

---

## STAGE 4 — Stretch Goal: Simple RL Agent (Simulated, No Hardware)
**Time: remaining budget before October. Only attempt after Stage 3 is solid.**

This connects directly to the compulsory *Reinforcement Learning for Bioengineers* module. Fully simulated — no hardware, no printer needed.

### Options
- **Option A — CartPole with a DQN in PyTorch, built from scratch** (not copied from a tutorial repo) using OpenAI Gym/Gymnasium
- **Option B — Simpler tabular Q-learning on a grid-world** first, before jumping to a full neural-network DQN — lower complexity, still teaches the Bellman equation and exploration/exploitation concepts directly
- **Option C — Skip DQN implementation entirely and instead do a short, honest write-up/notebook summarizing the RL concepts (MDP, Bellman equation, Q-learning) with small hand-built examples** — lower time cost, appropriate if you're running low on runway before October

**Honest recommendation:** if time is tight, **Option B then C**, not A. A tabular Q-learning agent on a grid-world teaches you the actual math (Bellman updates) more transparently than a DQN, which hides the concept behind neural-network machinery. Full DQN-from-scratch (Option A) is worth doing eventually, but only once RL for Bioengineers actually requires it — don't let it eat your last two weeks before you fly out.

### Resources
- **Gymnasium docs** (maintained fork of OpenAI Gym): https://gymnasium.farama.org/
- **Sutton & Barto, "Reinforcement Learning: An Introduction"** (free PDF, the field's standard reference): http://incompleteideas.net/book/the-book-2nd.html — read Ch. 1-3 for the Bellman equation and Q-learning fundamentals rather than the whole book

---

## Time Budget Summary

| Stage | Hours | Weeks |
|---|---|---|
| 0 — Git/GitHub | 2-4 | <1 |
| 1 — Python fluency | 10-15 | 1-2 |
| 2 — Serial bridge | ~10 | 1 |
| 3 — Closed-loop controller | 40-50 | 3-4 |
| 4 — RL stretch | remaining | remaining |

**Not in this document but still on your plate in parallel:** confirming electives, emailing Burdet with a specific project pitch, and a linear algebra/ODE refresher — none of those are coding tasks and none of them belong on this checklist, but don't let this project plan crowd them out. They matter at least as much as anything above.
