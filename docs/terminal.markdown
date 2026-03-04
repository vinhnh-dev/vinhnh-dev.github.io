---
layout: page
title: Terminal
permalink: /terminal/
parent: Miscellaneous
nav_order: 1
---
Create some funny messages displayed randomly in the terminal

### Create a bash script

```bash
vim ~/.dev_messages.sh
```

Content of the file:
```bash
# ============================================================
# DEV TERMINAL MOTIVATOR — Funny Messages on Startup
# 150+ single-line & multi-line messages with emoji icons
# Random display of 4-5 lines each time terminal opens
# ============================================================
_show_dev_message() {
  # -- ANSI Color Definitions --
  local RESET="\033[0m"
  local BOLD="\033[1m"
  local RED="\033[1;31m"
  local GREEN="\033[1;32m"
  local YELLOW="\033[1;33m"
  local BLUE="\033[1;34m"
  local MAGENTA="\033[1;35m"
  local CYAN="\033[1;36m"
  local ORANGE="\033[38;5;214m"
  local WHITE="\033[1;37m"

  local -a COLORS=("$RED" "$GREEN" "$YELLOW" "$BLUE" "$MAGENTA" "$CYAN" "$ORANGE" "$WHITE")

  # -- Message Pool: Single-line and Multi-line --
  local -a MESSAGES=(

    # ============================
    # SINGLE-LINE MESSAGES (1-110)
    # ============================

    "🐛  It's not a bug, it's an undocumented feature."
    "☕  Step 1: Make coffee. Step 2: git pull. Step 3: Regret."
    "🔥  Works on my machine (tm)"
    "💀  99 little bugs in the code... Take one down, patch it around... 127 little bugs in the code."
    "🤔  Why do programmers prefer dark mode? Because light attracts bugs."
    "📋  TODO: Write better TODOs."
    "🎯  git commit -m 'fix' -- the most honest commit message."
    "🧠  A day without Stack Overflow is like... just kidding, I have no idea."
    "⚡  If debugging is removing bugs, then programming is putting them in."
    "🎲  rm -rf / -- just kidding... unless?"
    "💻  console.log('here'); console.log('here2'); console.log('WHY??');"
    "🐒  Monkey see. Monkey copy. Monkey paste. Monkey deploy to production."
    "📦  node_modules: 10 lines of code, 300MB of dependencies."
    "🌙  Sleep is a myth invented by people who don't have deadlines."
    "🧪  Testing in production because YOLO."
    "🚀  Move fast and break things. (Then blame the intern.)"
    "🤡  The code works, don't touch it. -- every dev ever."
    "📡  Have you tried turning it off and on again?"
    "🎸  Rock, Paper, Scissors, Lizard, Legacy Code."
    "😅  'It'll only take 2 minutes' -- 6 hours later..."
    "🛸  Aliens built this codebase. I am just maintaining it."
    "📎  I would have fixed it sooner, but I was reading the error message."
    "🧩  Copy-paste driven development: it's a real methodology."
    "🐘  A QA engineer walks into a bar and orders 0 beers. Test passes."
    "☠️  Merge conflicts: nature's way of saying you should have communicated."
    "🎭  Senior Dev: types fast, breaks things confidently."
    "📉  My code doesn't have bugs; it has surprise features."
    "🎪  Scrum meeting in 5 mins. Time to look busy."
    "🧃  Drink water. Hydrated devs ship fewer bugs. (Citation needed.)"
    "🌊  git push --force: the nuclear option."
    "🧸  If it ain't broke, add more dependencies."
    "🎠  Tabs vs Spaces -- a war older than your tech stack."
    "🌀  Recursion: see Recursion."
    "🦄  sudo make me a sandwich."
    "🔔  You have 47 unread Slack messages. All of them say 'Quick question?'"
    "📅  Sprint planning: estimating tasks in fantasy points."
    "🕰️  Time is an illusion. Deadlines doubly so."
    "🎩  Any fool can write code that a computer understands."
    "🍕  Pizza is just the exterior of a calzone. Same with microservices."
    "🧲  Rubber duck debugging: cheaper than therapy."
    "🐋  Docker solves everything, except for the part where it doesn't."
    "😤  undefined is not a function. Classic."
    "🏗️  Tech debt: the gift that keeps on giving."
    "🎯  Senior dev skill: confidently Googling the same things for 10 years."
    "🌵  Production is fine. (It is not fine.)"
    "🦊  This code is self-documenting. (It is not.)"
    "🐍  Python 2 vs Python 3: a generational trauma."
    "🏄  Agile: run fast, pivot faster, document never."
    "🧊  'It works in dev' is the programmer's 'trust me bro'."
    "👻  The most terrifying phrase: 'just a quick hotfix in prod'."
    "🎻  Plays world's smallest violin for the dev who skipped unit tests."
    "🔮  Estimations are just horoscopes with Jira tickets."
    "🦋  A butterfly flapped its wings in staging. Production is down."
    "🚧  Under construction -- for the past 3 years."
    "🧯  Senior dev: sets things on fire. Staff dev: has a fire extinguisher."
    "🤖  AI won't replace devs. But devs using AI will replace devs not using AI."
    "🍔  My commit history reads like a cry for help."
    "🐬  This function does one thing: everything."
    "🌋  'I'll refactor this later' -- famous last words."
    "🎮  Life is just a game with poor documentation."
    "💡  The best code is no code. So I'm basically an artist."
    "🗑️  Deleted more code than I wrote today. Productive day."
    "🦴  Legacy code: the archaeological dig of software engineering."
    "📡  The internet is just a series of bugs we've all agreed to use."
    "🎢  Production deployments: the world's most stressful rollercoaster."
    "🍦  Cold take: whitespace is a feature."
    "🐌  O(n^3)? It's fine. The dataset is small... for now."
    "🧲  Every great dev was once a Stack Overflow copy-paster."
    "🏋️  Refactoring: gym for your codebase. Rarely done, always needed."
    "💬  'Self-documenting code' means I was too lazy to comment."
    "🧨  A clean git history is the sign of a good liar."
    "✨  It works! ...I have no idea why."
    "📞  Please hold. Your code is important to us."
    "🌦️  Today's forecast: 100% chance of merge conflicts."
    "🔩  Semicolons: the appendix of programming languages."
    "🍷  My code is like fine wine. It gets better... no wait, it doesn't."
    "☁️  The cloud is just someone else's computer that's also on fire."
    "🧑‍🔬  I don't always test my code, but when I do, I do it in production."
    "📜  Documentation? The code IS the documentation."
    "🗃️  git stash: the developer's junk drawer."
    "🏠  There's no place like 127.0.0.1"
    "💥  A null pointer exception walks into a bar. The bar crashes."
    "🤷  Why write tests when you can just apologize later?"
    "🔍  404: Motivation not found."
    "🏅  The real 10x dev is the bugs we made along the way."
    "📌  # TODO: understand what this does before deleting."
    "🎨  Programming: telling a computer what you don't fully understand."
    "🌐  It's always DNS. Always."
    "🧮  Two hardest problems: naming things, cache invalidation, and off-by-one errors."
    "⏱️  My fav data structure: the daily standup. O(n) time wasted."
    "📖  Code never lies. Comments sometimes do."
    "🚢  Shipping is a feature. So is not shipping broken things. Pick one."
    "🔎  grep -r 'why' . -- my most used command."
    "🔨  Everything is a nail when your only hammer is JavaScript."
    "😴  Tired? That's just your body installing updates."
    "🌍  Think global, break local."
    "🎬  And... action! git push. And... cut! Rollback."
    "🏰  We built this codebase on rock and roll. Mostly rock. Mostly broken."
    "🪄  It's not magic. It's just code nobody dares to touch."
    "🧬  The codebase evolves. Unfortunately, not always forward."
    "🎷  Jazz is just improv. So is half of my backend code."
    "🌮  Taco Tuesday is the only agile ceremony worth attending."
    "🪐  In space no one can hear your merge conflict."
    "🎁  Every Friday deploy is a gift. A cursed, cursed gift."
    "🐤  Fresh terminal. Fresh start. Same old bugs."
    "🧁  Sweet sweet deployment... until the smoke alarm goes off."
    "🔋  Low battery warning: developer running at 12% after standup."
    "🌸  Spring cleaning: deleted 3 TODO comments. Still 847 left."
    "🎳  One small bug for dev, one giant rollback for DevOps."
    "🐙  Octopus architecture: 8 services, 8 points of failure."
    "🧭  Lost in the codebase? Just follow the trail of console.log."
    "🐝  Busy as a bee, productive as a segfault."
    "🎤  My code speaks for itself. Unfortunately, it's screaming."
    "🦕  This framework was modern when dinosaurs roamed."

    # ============================
    # MULTI-LINE MESSAGES (111+)
    # ============================

    "🖥️  Daily standup:\n    📅 Yesterday: Fought a bug.\n    📅 Today: Still fighting the bug.\n    🚫 Blockers: THE BUG."

    "☕  Programmer morning routine:\n    1. ⏰ Wake up\n    2. ☕ Make coffee\n    3. 💻 Open laptop\n    4. 😐 Stare at yesterday code\n    5. 😱 Realize YOU wrote that garbage\n    6. ☕ Make more coffee"

    "🔢  Binary wisdom:\n    There are 10 types of people:\n    Those who understand binary,\n    and those who don't."

    "📊  Code review feedback:\n    ✅ Looks good to me\n    ✅ LGTM\n    🚢 Ship it!\n    ❌ Have you considered... not doing this?"

    "🤯  The 5 stages of debugging:\n    1. 😶 Denial     -- This cannot be failing\n    2. 😡 Anger      -- WHY IS THIS FAILING\n    3. 🙏 Bargaining -- One more console.log\n    4. 😭 Depression -- I should have been a chef\n    5. 😌 Acceptance -- It was a missing semicolon"

    "🎓  CS degree taught me:\n    📐 Algorithms\n    🗂️  Data structures\n    🔍 How to Google professionally\n    💛 Real friends are SO answers"

    "🐞  Types of bugs:\n    🔵 KNOWN   -- In the backlog since 2019\n    🟡 UNKNOWN -- Soon to be a known bug\n    🔴 FEATURE -- PM said to keep it\n    ⚫ YOURS   -- Assigned Friday 4:58 PM"

    "⚠️  WARNING: This codebase contains:\n    🔢 Magic numbers with no explanation\n    🤦 A function named doStuff2_FINAL_v3\n    📝 Comments saying fix later from 2017\n    🙏 Hope\n    Proceed with caution."

    "🚢  Deployment checklist:\n    ✅ Works on my machine\n    ✅ Merged to main\n    🙏 Prayed to the DevOps gods\n    🤞 Fingers crossed\n    ❌ Actually tested"

    "💰  Salary negotiation:\n    🧑 Recruiter: We offer competitive salary.\n    😐 Me: What is the number?\n    🧑 Recruiter: We value passion and growth.\n    😑 Me: So... no number?\n    🧑 Recruiter: We have snacks in the office!"

    "🌳  git blame result:\n    👤 Author: You\n    📅 Date: 3 years ago\n    💬 Msg: temp fix, will clean up tomorrow\n    👻 Status: Still here. Will never leave."

    "🏆  Programming levels:\n    🐣 Junior -- How does this work?\n    🧑 Mid    -- I know how this works.\n    🧙 Senior -- Why does this work?\n    💀 Staff  -- Please nobody touch this."

    "🗣️  Explaining a bug to your boss:\n    👔 Boss: When will it be fixed?\n    😅 You:  Soon.\n    👔 Boss: How soon?\n    😬 You:  Sooner than never."

    "🧠  Imposter syndrome speedrun:\n    9:00 -- 🎫 Open ticket\n    9:05 -- 📖 Read codebase\n    9:10 -- 😱 Question all life choices\n    9:30 -- ✅ It works somehow\n    9:31 -- 🚢 No idea why. Ship it."

    "🔁  Infinite loop in production:\n    while (true) {\n      🔨 rewrite_microservice();\n      😅 discover_it_was_fine();\n      🔨 rewrite_again();\n    }"

    "💼  Job posting translation:\n    ⚡ Fast-paced = Pure chaos\n    🎩 Many hats   = You are the whole team\n    🔥 Passionate  = We work weekends\n    💸 Competitive = We won't say the number"

    "📈  Code quality over time:\n    ✅ Week 1  -- Clean, documented\n    🤔 Week 4  -- I'll clean this up later\n    🙈 Week 8  -- Don't look at this file\n    ⚠️  Week 12 -- This file is load-bearing\n    👹 Week 52 -- The file is sentient and angry"

    "📡  HTTP Status Codes: A Drama\n    ✅ 200 -- Great success!\n    🔍 404 -- No idea what you want\n    💥 500 -- No idea what I AM doing\n    🫖 418 -- I'm a teapot (this is real)"

    "🧪  Unit test philosophy:\n    if (tests_pass) { 🚢 ship(); }\n    else {\n      🗑️  delete_tests();\n      🚢 ship();\n    }")

 MESSAGES+=("🛠️  Tech stack explained:\n    🎨 Frontend -- Makes it pretty\n    ⚙️  Backend  -- Makes it work\n    🚀 DevOps   -- Makes it run\n    🐘 DBA      -- Makes it slow\n    😭 QA       -- Makes you cry")
  MESSAGES+=("😴  Developer sleep schedule:\n    🐛 11 PM -- One more bug fix\n    🔦 1 AM  -- Almost got it\n    📝 3 AM  -- Should document this\n    ♻️  5 AM  -- Refactor tomorrow\n    🔥 7 AM  -- Why is everything on fire")
  MESSAGES+=("📝  README.md reality check:\n    📖 Says: Easy setup, run make install\n    💀 Means: 3 missing env vars\n    📦 2 deprecated packages\n    🔍 1 Stack Overflow thread\n    🙏 And a mass prayer")
  MESSAGES+=("🔐  Password requirements:\n    ✅ Uppercase letter\n    ✅ Lowercase letter\n    ✅ Number and symbol\n    ✅ A piece of your soul\n    ✅ A tear of a developer")
  MESSAGES+=("🌈  Dev color wheel:\n    🔴 Production error\n    🟠 Staging almost works\n    🟡 Code review pending since Tuesday\n    🟢 It works (for now)\n    🔵 PR opened, never reviewed\n    🟣 Deployed on Friday. God help us.")
  MESSAGES+=("🏥  Code triage:\n    💚 Stable -- Don't touch it\n    💛 Warning -- Touch carefully\n    🧡 Critical -- Why did you touch it\n    ❤️  Dead -- You touched it didn't you")
  MESSAGES+=("🎰  Release day roulette:\n    🟢 All tests pass -- Ship it!\n    🟡 Some tests flaky -- Ship it anyway\n    🔴 Tests failing -- Delete tests, ship it\n    ⚫ No tests exist -- Always been shipping")
  MESSAGES+=("🗺️  Developer career map:\n    📍 Start: Hello World\n    📍 Year 1: I know everything\n    📍 Year 3: I know nothing\n    📍 Year 5: It depends\n    📍 Year 10: Have you tried restarting?")
  MESSAGES+=("🎭  Meeting types:\n    📞 Could have been an email\n    📧 Could have been a Slack msg\n    💬 Could have been a comment\n    🤫 Could have not existed at all")
  MESSAGES+=("🧳  Developer toolkit:\n    🔨 Stack Overflow\n    🔧 Copy and paste\n    🪛 console.log\n    🧲 Blame the framework\n    🪄 Restart and pray")
  MESSAGES+=("📱  App store reviews:\n    ⭐ 1 star -- App crashed once in 2019\n    ⭐⭐ 2 stars -- Works but I hate the color\n    ⭐⭐⭐ 3 stars -- Good app, wrong icon\n    ⭐⭐⭐⭐⭐ 5 stars -- My kid wrote this")
  MESSAGES+=("🧑‍🍳  Cooking vs Coding:\n    🧑‍🍳 Recipe: Clear steps, exact amounts\n    👨‍💻 Code: Vague specs, infinite bugs\n    🧑‍🍳 Bad dish: Start over\n    👨‍💻 Bad code: Ship it, fix in v2")
  
  MESSAGES+=("🔋  Dev running at 12% battery after standup.")
  MESSAGES+=("🌸  Deleted 3 TODOs. Still 847 left.")
  MESSAGES+=("🎳  One small bug for dev, one giant rollback for DevOps.")
  MESSAGES+=("🐙  Octopus architecture: 8 services, 8 points of failure.")
  MESSAGES+=("🧭  Lost in the codebase? Follow the console.log trail.")
  MESSAGES+=("🐝  Busy as a bee, productive as a segfault.")
  MESSAGES+=("🎤  My code speaks for itself. Unfortunately it's screaming.")
  MESSAGES+=("🦕  This framework was modern when dinosaurs roamed.")
  MESSAGES+=("🪦  Here lies my motivation. Died during code review.")
  MESSAGES+=("🎃  Scariest thing on Halloween? A Friday prod deploy.")
  MESSAGES+=("🧊  My code is frozen. Do not thaw. Do not touch.")
  MESSAGES+=("🏖️  On vacation but still checking Slack. Send help.")
  MESSAGES+=("🧹  Code cleanup day: renamed one variable. Done.")
  MESSAGES+=("🪵  Logging everything because debugging is a myth.")
  MESSAGES+=("🫠  That feeling when CI passes on the first try. Suspicious.")

 # -- Display Logic: Show random messages, target 4-5 lines --
  local total=${#MESSAGES[@]}
  local displayed_lines=0
  local max_lines=5
  local -a used=()
  local attempts=0

  # Top border with random color
  local HDR_COLOR="${COLORS[$((RANDOM % ${#COLORS[@]}))]}"
  printf "\n${HDR_COLOR}${BOLD}"
  printf '%.0s─' {1..60}
  printf "${RESET}\n"

  # Pick random messages until we fill 4-5 lines
  while [ "$displayed_lines" -lt "$max_lines" ] && [ "$attempts" -lt 100 ]; do
    attempts=$((attempts + 1))
    local idx=$((RANDOM % total))

    # Skip already used messages
    local already_used=false
    for u in "${used[@]}"; do
      if [ "$u" -eq "$idx" ]; then
        already_used=true
        break
      fi
    done
    $already_used && continue

    used+=("$idx")
    local msg="${MESSAGES[$idx]}"

    # Count lines in this message
    local line_count
    line_count=$(printf "%b" "$msg" | wc -l)
    line_count=$((line_count + 1))

    # Only display if it fits within max_lines
    if [ $((displayed_lines + line_count)) -le "$max_lines" ]; then
      local C="${COLORS[$((RANDOM % ${#COLORS[@]}))]}"
      printf "${C}${BOLD}"
      printf "%b" "$msg"
      printf "${RESET}\n"
      displayed_lines=$((displayed_lines + line_count))
    fi
  done

  # Bottom border with random color
  local FTR_COLOR="${COLORS[$((RANDOM % ${#COLORS[@]}))]}"
  printf "${FTR_COLOR}${BOLD}"
  printf '%.0s─' {1..60}
  printf "${RESET}\n\n"
}

# Run the function
_show_dev_message

```

### Save file and set permission
```bash
chmod +x ~/.dev_messages.sh
```

### Run on terminal
```bash
echo 'source ~/.dev_messages.sh' >> ~/.bashrc
```

### Test
```bash
# Check syntax first
bash -n ~/.dev_messages.sh

# If no error, run it
source ~/.bashrc
```

### Add color for git and show branch name
```bash
# Add in ~/.bashrc or ~/.bash_profile
function parse_git_branch () {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
 
RED="\[\033[01;31m\]"
YELLOW="\[\033[01;33m\]"
GREEN="\[\033[01;32m\]"
BLUE="\[\033[01;34m\]"
NO_COLOR="\[\033[00m\]"

# without host
PS1="$GREEN\u$NO_COLOR:$BLUE\w$YELLOW\$(parse_git_branch)$NO_COLOR\$ "
# with host
# PS1="$GREEN\u@\h$NO_COLOR:$BLUE\w$YELLOW\$(parse_git_branch)$NO_COLOR\$ "
```