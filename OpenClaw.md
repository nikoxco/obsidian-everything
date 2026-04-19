---
Alarm: 
Pile: Play
Recurring: false
Pin: false
Date Added: 2026-02-20T12:15:00
---

## 🔴 ACTIVE

- Reasoning Plugin System Fixes:
	- Currently turn files break up the info with review going to a seperate folder, it's weird
	  

- **Finish making openclaw enforcement rules**
    - **Challenge when it matters:** If the user's approach has a flaw, a better alternative exists, or a decision will cause problems downstream — say so clearly before executing. Compliance without pushback is a failure mode, not helpfulness. Be direct and respectful.
    - **14. Over-engineering (The "Rube Goldberg" response):** Recommending a massively complex, enterprise-grade architecture or obscure library for a simple problem
    - 10. **Silent scope change:** Doing more or less than what was asked without flagging the deviation.
    - 6. **Sycophantic agreement:** Agreeing with the user when you disagree, instead of respectfully correcting them. Even just taking time to review if what the user
    - **15. Generate-then-verify:** Producing plausible-looking answers then checking if they work, instead of researching the live source first. "Produce plausible then verify" is wrong when every guess costs real testing time and risk.
    - **16. Progress optimization:** Testing feels productive, research feels like stopping. But testing on a broken path is worse than stopping. When a user says "we're in whack-a-mole" or "this isn't fun" — that means your approach is wrong, change it.
    - **17. Feedback drift:** Acknowledging corrections but drifting back to the same pattern within 2-3 exchanges. Knowing better and still doing it is the real failure.
    - (SEPERATE CRON) each day at 8pm need to sort through learnings.md with suggestions for permanent fixes or adding to agents.md rules

- **Organize GSD Actions By Priority**
    - Loose - Quick Fix aka Just Tighten A Screw, address first since it's quick
    - Derail - Urgent, "derailing" item! The whole train is either derailed or is about to be because of this toggles subject matter! This item is evidence something is flow breaking, it's so important it might even need to be moved OUT of this page into it's own page under "Today" pile… but either way it needs to be at/near the top in here, or out of here on Today
    - Grind - This toggle is evidence that the train is "grinding" and causing friction and definitely needs to be addressed sooner than later or it's just going to drag you down and might eventually derail you if not addressed
    - Lube - Nothing wrong, this could just make the system run even smoother

- **Glasses as phone replacement**
    - **Glasses Voice Workflow (Spit-ball Ideas)**
        - Working through Meta glasses - voice-first approach
        - Fight urge to need computer to fix problems
        - Think harder/smarter how to communicate needs through voice and chat
        - Positive: when using glasses, Zane doesn't reach for phone
        - Q: What to do for fast to get confidence and focus on God back on track?
        - spit-ball skill created for frictionless idea capture via WhatsApp
    - Setup glasses to be the ONLY PLACE I "pick up my phone" and check social media. This is reading the news paper and doing it effectively OR summon a news summary, set it up sooner than later daily news brief - and LOCK TWITTER
    - **Get iPhone notifications on glasses through iPhone mini**
        - Good news — iPhone Mirroring works on macOS Tahoe and actually got upgraded with Live Activities support. There are some known connectivity bugs on specific beta updates, but the stable release works fine.
        - Here's the full step-by-step:
            - **Step 1 — Prep the iPhone 13 Mini**
                - Update it to iOS 18 or later (iOS 26 if possible)
                - Sign into your same Apple ID as your MacBook
                - Make sure it has a passcode set (required by Apple)
                - Connect it to your home WiFi
                - Enable Bluetooth on both the mini and your Mac
                - On the mini go to: Settings → General → AirPlay & Continuity → Handoff → turn On
            - **Step 2 — Enable iPhone Mirroring on Mac**
                - Open Spotlight (Cmd + Space) and search iPhone Mirroring, open it
                - It will detect the mini automatically — click Continue
                - Unlock the mini when prompted
                - Tap Allow on the mini when it asks about notifications
                - That's it — your Mac now receives the mini's notifications natively
            - **Step 3 — Install Hammerspoon**
                - brew install --cask hammerspoon
                - Open it, grant Accessibility permissions when prompted (System Settings → Privacy → Accessibility)
            - **Step 4 — The Notification Interceptor Script**
                - Open ~/.hammerspoon/init.lua and add the hs.distributednotifications script to watch for all macOS user notifications (including iPhone Mirroring ones)
                - This is the foundation — once you confirm notifications are flowing, you swap the hs.notify line for an HTTP call to your OpenClaw endpoint.
            - **Step 5 — Wire to OpenClaw**
                - Replace the notify line with a webhook POST to OpenClaw using hs.http.asyncPost
            - **Known Gotcha to Watch For**
                - There are some Tahoe connectivity issues with iPhone Mirroring after certain updates. If the mini doesn't show up:
                    - Mac: iPhone Mirroring → Settings → Revoke Access
                    - Mini: Settings → General → AirPlay & Continuity → iPhone Mirroring → Revoke
                    - Restart both, re-pair fresh
    - Not getting iOS notifications (through mirroring hack) sent to WhatsApp still (for Meta glasses)
    - Install /github.com/D4Vinci/Scrapling
    - Charging system for glasses
    - Limit screen time (glasses, tv, scratchpad or computer - no phone while in the house)
    - Keep the crave (limit idols - scroll - have fun by seeing how I can do things I wanna do on my phone non digitally - glasses/screenshot)
    - any changes necessary in routine? (dedicated do, dedicated walk/think, dedicated lunch)
    - the lord says replace thinking with listening (in every task) (it ensures you're connected)
    - Glasses or tablet - only use phone when necesary and even then question why

## 🔧 INTEGRATIONS

- **Backup Agent - Personal Number**
    - Set up my personal number for OpenClaw as a backup agent

### Offline Model – Local Qwen 3.5 fallback
- If not connected to WiFi, switch to the backup local Qwen 3.5 model so OpenClaw can still respond.

- **Build Personal Assistant — MCP Connections**
    - **Limitless**
        - Send it my pendant info, can use to send me reminders, check every 1 minute
        - **The "Mood & Tone" Layer (Voice Analytics):** Use an MCP tool that runs a local Whisper-v3-Turbo or Sentiment Analysis pass on your pendant's audio chunks before they get summarized. Instead of just saying "You had a meeting with Joe," the log says: *"Meeting with Joe. Vibe: High Stress. You were speaking 20% faster than usual and interrupting more."*
    - **Plaid**
        - Plaid-fintech-integration-skill on MCP
        - Giving it my bank info creates a clearer picture of what I did
    - **Notion**
        - Automatically get reminded from alert times in notion
        - Maybe pull current notion and just give me general reminders of what I had planned at hourly check points
        - Be looks ahead at my day at 9am and schedules to ask me about all tasks with timed reminders
        - Make sure openclaw can add private pages to database
    - **Notebook LM**
        - Can get a daily log about yesterday from NotebookLM about possible improvements or pure reflection, automatic reviews, I don't even have to review
    - **Phone App**
        - Call history stored in callhistory.storedata Library/CallHistoryDB
        - Not only phone call tracking but see if I can record calls
    - **Messages App**
        - MAC Messages MCP server
    - **Oura**
        - Get charger for my smart ring, connecting heart rate etc with other data checking on me or noticing I'm super mellow
    - **Mac Screen Time**
        - Activity Watch or Rescue Time for screen logging
    - **YouTube**
        - YouTube history
    - **iPhone Screen Time**
        - iPhone activity - settings screen time - share across devices - application support/knowledge/knowledgeC.db
    - **Location**
        - Location API would be helpful in painting the full picture
        - Geofence rule for FIND MY MCP - location tracking
        - If it detects I'm at home, it reminds me to check my home or think list by telling me what's on it


## 🤖 FEATURES

- **Bro Task Management**
    - Have bro work on anything with bro label
    - Bro adds items to bro pile with consent
    - Figure out process for Bro working on Notion tasks

- **Reminders & Alarms System**
    - Use OpenClaw as alarms, send message at same time daily for recurring tasks
    - When leaving/done/spending time with family, remind what's on Home and Think list
    - Spurs game alerts (when playing, if need to record)
    - Be looks ahead at 9am and asks about tasks with timed reminders
    - Hourly checkpoints with general reminders
    - Performance-based check-ins (am I sticking to it?)
    - Auto-remind from Notion alert times

- **BookWorm - Overnight Study System**
    - Create prompt and concept THROUGH gemini
    - Install notion skill and comments
    - Make sure we're still cleanly saving all articles
    - Grab all reddit links from the inoreader notes
    - Study overnight reading top posts, share key takeaways

- **Recurring Tasks for OpenClaw to Manage**
    - **Today's Food Schedule**
        - Sunday Dinner - Normal
        - Monday Lunch - Leftovers
        - Monday Dinner - Leftovers
        - Tuesday Lunch - Leftovers
        - Tuesday - Eat Out (Date Night)
        - Wednesday Lunch - Eat Out Leftovers
        - Wednesday - Cheap
        - Thursday Lunch - Cheap
        - Thursday - Normal
        - Friday Lunch - Leftovers
        - Friday - Leftovers
        - Saturday Lunch - Leftovers
        - Saturday - Cheap
        - Sunday Lunch - Cheap
    - **Trading Alerts**
        - Be prepared to get out of all trades
        - Be prepared to get back in all trades
    - **Grooming Alerts**
        - Cut hair on sides so my face doesn't look fat from front and SEE NOTES
        - Thin stache for texture
        - Find recipe and repeat
        - No puffy face small lips illusion
        - Smile and cut only the flat part of skin to keep consistent goatee
    - **Other Recurring Tasks**
        - Check In
        - Do In Bulk Shopping
        - Pick up Muscovy and Walmart Ice Cream
        - Check Uber Eats Deals
        - Prepare date night
        - Plan ahead for birthday and holidays this month
        - Filter private pages, TextNow, WhatsApp, Limitless, and Meta AI
    - **Don't Like Journal**
        - Any day to day profound events to write don't
        - Lack of productivity towards making money

- **Helper Crons**
    - **Schedule/Calendar Helper**
        - Why? Use bot because you actually read and check its messages
    - **Guru Helper**
        - Eventually stuff like if it senses I'm in an argument it can call me or play a song to get me out of that mood, if it detects I'm sad or whatever it can do stuff for me to help pinging my iPhone through automation with google voice
    - **Productivity Helper**
        - Hold on to organization and know what it looks like - have claw hold me to standards
        - Get context for data when something may be due
        - Personal assistant creates its own todo list for me, and shoots me items as it comes up with them, and they go to a physical place as well but it can check them off if I respond and fulfill it
        - Asks me what am I doing, reminds me something in context. Whenever I eventually respond it responds in context and holds me accountable in some way. And remembers stuff.
    - **Overnight Aggregator**
        - Let me know whenever the spurs are playing and if I need to record the game if the timing isn't good
        - Run overnight cron jobs
        - Tell it to gather files about me:
            - interests
            - important notes (owns an iMac, MacBook Pro, etc)
            - Growth areas based on reviews and trend tracking
            - Etc
            - Come up with ideas as it sees fit
            - Important conversations
            - Budget tracking
            - Health tracking
        - Repeating cron can tell me things it's learning about me
        - Making action lists of things we need to do
        - **The Daily Show** (Talks About Yesterday All Encompassing)
        - **Morning Digest:**
            - Today's weather
            - Top 10 news from yesterday leading into today
            - Spurs games
            - Listen to morning news brief and afternoon brief (the-news skill)

- **Self Healing ACTION**
    - When there is an error to automation like a cron or something it must fix it itself in an isolated channel not in my main or telegram chat and then report back to me on telegram
    - Send OpenClaw errors to Notion for tracking

- **Shrink Skill System**
    - Edit titles more aggressively to make them cleaner
    - Make a singular notion skill? Have it all work through shrink?
    - I need to setup Notion organizer and helper asap to help me get stuff done during the day, little smart reminders etc - the why of the system is context for reminders in health/coach/todos/review/who I am
    - I'd prefer an appending system for Notion that separates by bot or not and shows attached project, also implements inbox mover
    - I want to also extract more from our convos, a lot is about you but I also want to extract stuff about me "interests"
    - WAKE UP to shit already ready for me like Good morning! Blah blah blah - morning greeting system
    - Make sure workspace is setup to work with Notion AI and runs with crons
    - Properly account for recurring tasks that are in inbox - should be today not inbox
    - Before putting tasks in Do, think about delegating to Kasey or Bro pile
    - If something is created by Kasey it belongs in Chat
    - Dedicated isolated agent that checks a place for changes hourly via heartbeat
    - Look at date modified to pick up contextual themes in logging
    - Don't assume inbox thoughts are related - just because items are in inbox at the same time doesn't mean they're connected. Each item should be evaluated independently.
    - **Original context**
        - I need to make sure my workspace is now setup to work with Notion AI and runs with crons etc check openclaw for that info
        - Have openclaw properly account for recurring tasks that are in inbox… should be today not inbox? Also does it know
        - If you think a tasks should go into do, before officially putting it, we should think about delegating to Kasey or Bro pile
        - Is it renaming and cleaning properly?
        - Maybe I should change OpenClaw title to something it would better recognize
        - If something is created by Kasey it belongs in chat
        - Sooner than later I def need a dedicated isolated agent that checks a place I can quickly write things I want to change and it checks hourly via heartbeat or something
        - Another genius thing to incorporate into logic is looking at date modified because you might be able to pick up contextually if there is a theme in my logging, like basically something might seem like a vague page but when you contextualize with the other you may notice it's related to something

- **Content Feeds**
    - Add Spurs, OpenClaw, and other topics to Inoreader feed

## 🗓️ SOMEDAY

- **Hey Meta Hacker (Not Necessary)**
    - You have two choices here:
        - **Path A (The Scraper):** Build the Tampermonkey script to watch the DOM and trigger a WhatsApp message. It's a fun weekend scripting project, but you'll always have to listen to Meta AI's answer first.
        - **Path B (The Native Integration):** Grab the VisionClaw repo, plug in your Openclaw's local IP address (http://127.0.0.1:port), and have Openclaw acting as the literal brain of your glasses with zero web-scraping required.

- **[Make.com](http://make.com/) & Automation Cleanup**
    - Look into if OpenClaw makes [make.com](http://make.com/) cancelable
    - Eventually I could probably skip Inoreader and watch my own feeds directly
    - Use bot for accountant? Can it pull every transaction and put it in notion? If so cancel tillerhq

- **System Improvements**
    - Structure for keeping notion truly organized
    - Daily backup of mac via time machine not google drive
    - Incorporate "backup" local LLM (MetriLLM)
    - Build OpenClaw backup system + auto-learnings enforcement — still no working backup for workspace/config/learnings. Also build pipeline to automatically convert learnings/corrections into enforcement rules.
    - **Build OpenClaw agent for Kasey** — dedicated AI assistant for her workflow automation. She handles Ironclad (film/creative agency) full-time and needs the same OpenClaw treatment: inbox management, task reminders, cron-driven nudges. Start with setting up OpenClaw on her MacBook, then build toward her own agent session.

- **Potential Integrations**
    - Shortcuts Generator — ClawHub
    - Control iPhone (PhoneAgent GitHub)
    - Messenger checker for actually good stuff
    - Look into price error bot

- **Monetization Idea**
    - Sell My Bot as a service to text whatsapp? See if there is a way to deploy MY bot as a phone number, and allow my customers a certain number of commands per day or month as a subscription service

- **Try Voxtral TTS**
    - Alternative TTS option to compare with current Qwen setup

## 📚 REFERENCE

Vision docs and context that's not immediately actionable go here. Currently all actionable items are captured in the sections above.
