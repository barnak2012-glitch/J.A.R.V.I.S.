# JARVIS - Desktop AI Assistant

Blue animated HUD, "Hey Jarvis" wake word, voice in/out, typed commands, document upload, Office file creation,
3D model generation, app launching, system control, web search.

Files: `jarvis.py` (main app), `skills.py` (system control), `model3d.py` (3D models), `requirements.txt`.
Keep all of them in the same folder.

## Setup (VS Code)
1. Install Python 3.10+ and open this folder in VS Code.
2. Terminal -> `python -m venv venv` then activate it
   - Windows: `venv\Scripts\activate`
   - Mac/Linux: `source venv/bin/activate`
3. `pip install -r requirements.txt`
   - If PyAudio fails on Windows: `pip install pipwin` then `pipwin install pyaudio`
   - Mac: `brew install portaudio` first. Linux: `sudo apt install portaudio19-dev python3-tk espeak`
4. For full intelligence set your Anthropic key (https://console.anthropic.com):
   - Windows: `setx ANTHROPIC_API_KEY "your-key"` (restart VS Code)
   - Mac/Linux: `export ANTHROPIC_API_KEY="your-key"`
5. `python jarvis.py`

Without a key it still works in lite mode (Wikipedia/DuckDuckGo answers, basic documents, preset 3D models).

## Wake word
Say "Hey Jarvis" then your command ("Hey Jarvis, open Chrome"). After he answers, he keeps listening for
25 seconds so you can follow up without repeating the wake word. The WAKE button turns this on/off.

## Things you can say or type
**Apps and web**
- "Open Chrome" / "Open Notepad" / "Open any installed app by name" / "Open youtube.com"
- "List apps", "Close Spotify" (asks you to confirm)
- "Search Google for best laptops" / "Play Arijit Singh on YouTube" / "Research black holes"

**Documents**
- "Create a Word document about climate change" / "Create an Excel sheet for monthly budget" / "Create a PowerPoint about AI"
- UPLOAD button for PDF/Word/Excel/PPT, then ask "Summarize my document"
- "Find file resume" (searches Documents, Desktop, Downloads...)

**3D models**
- "Create a 3D model of a robot" / "Generate a 3D model of a castle"
- It spins on the HUD, opens an interactive viewer in your browser, and saves GLB, STL (3D printing) and OBJ
  files in `Documents/Jarvis_Output/3D`. "Hide model" clears it from the HUD.
- With an API key Claude designs any object. Without one: house, table, robot, tree, rocket, snowman, cube, sphere...

**System control**
- "Volume up / down / mute", "Set brightness to 60"
- "Take a screenshot", "Type hello world", "Press control c", "Scroll down", "Click"
- "Battery status", "System status", "Weather in Mumbai"
- "Lock screen"; "Shutdown / restart / sleep" and "Run command ..." ask you to say "confirm" first

**Productivity**
- "Remind me in 10 minutes to call mom" / "Set a timer for 5 minutes"
- "Note down buy milk" / "Read my notes"
- "Send WhatsApp message to 919876543210 saying hello" / "Email to name@gmail.com saying hi" (opens the app with the draft; you press send)
- "What can you do?", "Clear" resets memory, "Goodbye" exits

Created files are saved to `Documents/Jarvis_Output` (OUTPUT button opens it).

## Screen reading and control (needs ANTHROPIC_API_KEY)
- **Read:** "What's on my screen?", "Read my screen", "Explain what I'm looking at". JARVIS minimizes himself, takes a
  screenshot, and answers by voice.
- **Control:** "Click the Save button", "On my screen, open Notepad and type hello", "Take control and search
  for lofi music on YouTube". He minimizes, then repeats: look at the screen, do one click/type/key/scroll, until done
  (max 12 steps).
- **Safety:** he always asks you to say "confirm" first. Say "stop" or move the mouse to any screen corner to abort.
  He is told never to enter passwords or payment details, or to confirm purchases, deletions or sent messages
  unless you explicitly asked.
- **Privacy:** each screenshot is sent to the Anthropic API for analysis. Close anything sensitive first.

## Voice
JARVIS speaks with a neural voice (free, needs internet), British male by default. If that's unavailable he
falls back to your system's offline voice.
- "Change voice to British / American / Indian / Australian", add "female" for a female voice
- Or any Microsoft voice name: "Change voice to en-IN-NeerjaNeural"
- "List voices", "Speak faster", "Speak slower", "Stop talking"
- Your choice is remembered in `Documents/Jarvis_Output/voice.json`

**Your own custom/cloned voice (optional):** create a voice at elevenlabs.io, then set two environment
variables and restart: `ELEVENLABS_API_KEY` and `ELEVENLABS_VOICE_ID`. JARVIS will use it automatically.
(ElevenLabs is a paid/limited-free service.)

## Safety
Anything destructive (shutdown, restart, sleep, closing apps, running shell commands) waits for you to say
"confirm". Keep this in mind, because voice recognition can mishear. Typing/clicking uses pyautogui; moving the
mouse to a screen corner aborts it.
