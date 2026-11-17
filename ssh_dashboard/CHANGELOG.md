# What's new

## 0.42.0

- Energy: the forecast for the month now includes the electricity company's meter fee (15 SAR plus VAT by
  default; it can be changed in Settings → Home).

## 0.41.0

- Names: every room and device can have an Arabic and an English name. Each person sees the names in their
  own app language, and alerts use the reader's language. Existing names are kept.

## 0.40.10

- Energy: a car charger has its own line under "By room", instead of being counted as "Not measured".

## 0.40.9

- Security: camera pictures can no longer be opened from the home network without signing in to the app.

## 0.40.8

- Water: correcting a tank's full depth no longer needs its history cleared.

## 0.40.7

- Water: a tank's level is measured from the water's depth, so it is right even when the sensor's own
  percentage isn't set up.

## 0.40.6

- Rooms: scripts sit with the scenes, and automations have their own section with an on/off switch.

## 0.40.5

- Devices: a tap switches a light, fan or switch; press and hold opens its controls.
- Lights, fans and switches that only turn on and off get one large switch in their controls.
- TVs: the volume buttons line up.

## 0.40.4

- Notifications: system restarts no longer appear in the inbox.
- Clearer wording on empty pages and in settings.

## 0.40.3

- Water: clearing a tank's history asks for your code first.

## 0.40.2

- Athan: while it plays, Stop also shows inside any device's controls, including the speaker playing it.

## 0.40.1

- Water: a tank's settings now have Clear history, to start its figures again from now, for example once
  its sensor is fixed in the tank.

## 0.40.0

- Usage: Energy is now Usage, with Electricity and Water side by side. Water shows how many days of water
  are left, water used today and each day of the week, each tank's level over the week, and when the tanks
  were last filled. It works from tank level sensors, or from a water meter if the home has one.
- Water tanks: a tank's level sensor shows as one card in its room, with the level, litres when the
  tank's capacity is set (in the sensor's settings), and whether it is low. A low tank also shows on Home.

## 0.39.8

- A lock's recent activity shows who locked or unlocked it from the app. Unlocking at the door (fingerprint,
  code, key) shows no name, because the lock doesn't tell the system who it was.

## 0.39.7

- A door sensor linked to a lock is called by the lock's name everywhere: in the alarm's "Open now" and
  "Close these first", on Home, and in the alarm alert.

## 0.39.6

- Disarm with the same code you sign in with. An administrator can turn on "Ask for the code to disarm"
  in Settings → Users & access; arming stays one tap. Five wrong codes pause that person for a few
  minutes, for signing in too.

## 0.39.5

- The alarm says which doors are open, and why a mode won't arm ("Close these first").
- Alarm codes: an administrator can give each person a code for disarming, and turn on "Ask for a
  code to disarm". Five wrong codes pause disarming for 5 minutes.
- On the way in, 30 seconds to disarm (the usual alarm standard), in every mode.

## 0.39.4

- While the alarm is going off, the Security page shows Disarm.
- The alarm alert reaches each phone in the language that phone's app uses (with app 1.0.61).

## 0.39.3

- Radio stations show their logos.
- Four stations that had stopped playing are back: Mishary Alafasy, BBC Arabic, Monte Carlo
  Doualiya and Saudia Radio (now on 96.2).

## 0.39.2

- Each album in My music shows how many tracks it has.

## 0.39.1

- Albums open: tap one in My music to see its tracks, play the whole album, shuffle it, or start
  from any track.

## 0.39.0

- Radio on Home: a new Music section under Scenes with your favourite stations. One tap plays on
  your speaker. Star the stations you want on the Music page.
- A cleaner Music page: what's playing and the speaker at the top, then two tabs, Radio and
  My music. Stations are small tiles by group, your albums show their covers, and one Shuffle
  all button plays everything.

## 0.38.0

- One alarm for the home. The Security page shows the home's alarm alone, with Away, Home and
  Night, and everyone in the house can arm and disarm it.
- When the alarm goes off, every phone gets a loud alert saying which door set it off.

## 0.37.1

- While the Athan plays, the volume on the Athan page changes it straight away (Sonos speakers).
  Each speaker goes back to its own volume afterwards.

## 0.37.0

- The Athan can be stopped: a bar at the top of every screen while it plays, with a Stop button,
  for anyone who can use that speaker. Music carries on afterwards.
- Prayer times in 12-hour form, and a new look for the Athan page.

## 0.36.0

- New for administrators: the Athan at prayer times. Settings → Athan & sounds shows today's
  times for your home's location, by the Umm al-Qura calendar. Choose the speakers, the volume and
  the prayers. Music on those speakers pauses for the Athan and carries on afterwards.
- Your choices on that page are now kept.

## 0.35.0

- New for administrators: Settings → Athan & sounds. Play a test sound on the speakers you choose.
  It plays over any music, which then carries on at its own volume. Athan times come next.

## 0.34.0

- Notifications are now personal. Each person chooses the devices they follow (Settings →
  Notifications), and hears about those devices only. Danger alerts, such as smoke, gas, water
  leaks or the alarm, still reach everyone. Low-battery notices go to administrators.
- Administrators keep the devices they were already notified about.

## 0.33.1

- Music: a speaker placed in a room can now play music and radio for everyone given that room.
  Speakers that can't play music from the app (such as Echo) are no longer offered.

## 0.33.0

- New for administrators: Settings → Automations. Turn any of your home's automations on or off,
  and see when each one last ran. To change what an automation does, use Home Assistant.
- Tighter checks on device commands. Nothing else changes on screen.

## 0.32.2

- Maintenance: tighter checks on who may operate a camera's own settings. Nothing changes on
  screen.

## 0.31.3

- Phone presence (home / away) is ready for a coming Home Assistant change. Each account that
  shares its location now appears in Home Assistant as a device named "SSH – <name>". Nothing
  changes in the app.

## 0.31.2

- Maintenance: the add-on is packaged more compactly. Nothing changes on screen.

## 0.31.1

- "Lights off" in a room now switches off every light there, including one that a wall
  switch failed to report as on.

## 0.31.0

- Choose which device gives each room its temperature and humidity: Settings → Rooms. Left on
  Automatic, nothing changes. If the chosen device stops reading, Automatic takes over until it
  is back.

## 0.30.9

- The music controls no longer disappear when you pause.
- A repeat button on the Music page: repeat everything, or just this track. With it on, Next on
  the last track starts again from the first.
- On the phone, the music bar has a back button.

## 0.30.8

- After you pause or the music stops, its controls stay at the bottom so you can carry on. Close
  them with the × (or swipe them away on the phone); they also go by themselves after two hours.
- Tapping the song in the music bar opens the Music page.

## 0.30.7

- While music plays, its controls stay at the bottom of every page, so you can pause or skip
  from anywhere.

## 0.30.6

- You stay signed in while you use the dashboard, and an update no longer signs you out.
- The Music page remembers your speaker and shows play and pause straight away.

## 0.30.5

- A new installation now shows all your devices as soon as the first account is created.
- On a shared screen, the next person to sign in now sees only what their own account allows.
- Live updates now stop as soon as someone signs out or a phone is removed.

## 0.30.4

- Your favourites are now safe if the connection drops while the page is opening.

## 0.30.3

- Pages fit better on a phone-sized browser.

## 0.30.2

- The Music page no longer offers speakers that are switched off.

## 0.30.1

- The music bar no longer hides behind the left menu when the menu is pinned open.
- Empty music lists no longer appear on the Music page.

## 0.30.0

- Radio is now on the Music page: Holy Quran, news and Arabic music stations, with no
  subscription.

## 0.29.6

- Behind-the-scenes improvements.

## 0.29.5

- Saving a long camera clip is more reliable on a slow connection.

## 0.29.4

- Phone notifications now have one switch: on means every alert your account can see.

## 0.29.3

- Behind-the-scenes improvements to how updates are delivered.

## 0.29.2

- Behind-the-scenes improvements to how updates are delivered.

## 0.29.1

- Everyone in the home can now see the door, window, smoke, gas and leak sensors on the
  Security page, as they already could the locks.
- Administrators can save a 1, 5 or 10 minute clip from a camera recording.
