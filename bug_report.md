# Lemon Clicker Bug Report

**Name:**
**Tickets solved:** <!-- e.g. 1, 2, 3, 5, 6 (at least five) --> 1, 2, 3, 5, 6

Copy the template below once for each ticket you solved. 

---

## Ticket <1>: <ticket Start Screen - Stats always say "Stats unavailable">

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->
     

**File / function:**
    MenuScreenController.td --> loadStatsFromServer()

**What was wrong:**
    fetch ("/api/stat") should have been fetch("/api/stats")

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**
    I was looking at the sections within MenuScreenController because that is the section that was being affected. Since it was stats I decided to see all the stat mentions in the file. I hadn't checked the api names for the api/stats, however, when I saw "/api/stat" in the non-plural sense, I though it sounded strange, so I decided to make it plural to see if that would do anything and low annd behold, the stats became available. --> Also!! I used tools and saw there was an issue in the console with the api call so that's why I knew to look at MenuScreenController.

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** "Stats Unavailable" on Menu screen
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log --> Devtools Console
3. **Traced back to:** MenuScreenContoller --> Traced back to api call
4. **Buggy code:** <!-- the file, function and line where the trace ended -->
    MenuScreenController.td --> loadStatsFromServer() --> fetch ("/api/stat") 



<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
The stats appeared on the menu screen

# Lemon Clicker Bug Report



---

## Ticket <2>: <ticket Start Screen - Best score always shows 0>

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**
    index.ts --> app.get("/api/stats", (_req, res) )

**What was wrong:**
    best was initialled to zero, the code was set up so that best was only updated if the entry score was lower than best, thus best wasn't being updated. 
		


### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?
    (entries[i].score < best) --> (entries[i].score > best) 
           
#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**
    Since average and the other stat were working, I just assumed there was something wrong with best and being updated. So I just looked for the code where best was initialized and calculated and checked the code there.



### Confirming the fix

<!-- How did you confirm the problem is gone? -->
    The best stat appeared updated in the stats section.

# Lemon Clicker Bug Report


---

## Ticket <3>: <ticket Gameplay- The lemon only ands in four spots>

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->
     

**File / function:**
    GameScreenView.ts --> randomizeLemonPosition()
**What was wrong:**
    Essentially, the code was rounding the randomized number. I'm assuming there were only two ways in which the number was rounded. It's like if the only possible values were 0 and 10. If you got anything lower than five, it would round you down to 0. Higher than five and you would get up to 10. This means that for x and y there were only two possible values for each, thus making it four total locations. 

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?


#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** <!-- what you saw, e.g. text on screen, console error --> The lemon was only moving to four possible positions when you clicked, thus, there was something wrong with the click or randomization. 
2. **Traced back to:** <!-- where that came from, e.g. a function, a request in the Network tab, a server log --> GameScreenController.ts --> private handleLemonClick() --> this.view.randomizeLemonPosition();
3. **Traced back to:** GameScreenView.ts --> randomizeLemonPosition()
4. **Buggy code:** <!-- the file, function and line where the trace ended -->
    GameScreenView.ts --> randomizeLemonPosition() --> 
        const randomX = Math.round(Math.random()) * (maxX - minX) + minX;
		const randomY = Math.round(Math.random()) * (maxY - minY) + minY;


<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your 
    reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
    I replayed the game and was able to confirm it is no longer just the four possible positions. 

# Lemon Clicker Bug Report



---

## Ticket <5>: <ticket Game over screen - "Leaderboard dates show 'undefined'">

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**
    ResultsScreenView.ts --> updateLeaderboard(entries: LeaderboardEntry[])
    ResultsScreenModel.ts --> export type LeaderboardEntry...

**What was wrong:**

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**
    I had previously looked at the leaderboard.json file and the time stamps were being recorded so I knew there wasn't an issue with the declaration of the time stamp. I decided to check where the leaderboard was being called up and realized that timestamp was named "timestamp" and not time like it's declared in the api call. It would have been easy changing the api call but I decided to change the variable timestamp to "time" and that worked. 

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
    The "undefined" no longer shows up --> Insted we see time and date. 
# Lemon Clicker Bug Report



---

## Ticket <6>: <ticket Game Over Screen - "My highscore disappeared from the leaderboard">

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**
    index.ts --> app.post("/api/leaderboard", (req, res))

**What was wrong:**
    Esentially, the way the scores was sorted was backwards. It was sorting in increasing order instead of decreasing. 

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?
    index.ts --> app.post("/api/leaderboard", (req, res)) --> 
    entries.sort((a, b) => a.score - b.score);  --> entries.sort((a, b) => b.score - a.score); 

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**
    Since I knew the sorting was wrong, I decided to switch b.score-a.score because it would create the correct logic. 



### Confirming the fix

<!-- How did you confirm the problem is gone? -->
    The highest scores were now on the top five and lower scores were not shown. 
