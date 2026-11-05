---
name: analyze-activity
description: Analyze a completed run, ride, or swim in depth. Use when the user asks to analyze a workout or activity, asks how their run/ride/swim went, or wants a breakdown of pacing, power, heart rate, or execution for a session. Defaults to the most recent activity if none is specified. Do not use for daily readiness questions (use checkin) or for planning future workouts (use replan).
---

Analyze one completed activity in depth and compare it against the user's recent training.

Finding the activity:

1. Determine the sport and which activity to analyze. If the user named a date or activity, use that. If they said "my last activity" or gave no specifics, fetch recent activities with `getRunningActivity`, `getCyclingActivity`, and/or `getSwimmingActivity` (limit to the sports the user actually does if known from context) and pick the most recent one.
2. For the computed numbers (normalized power, intensity factor, variability, time-in-zone, pacing/fade first vs second half, avg/max/min of power, HR, cadence, pace, and equal time-window splits), call `analyzeActivityStream` with the activity's id — it computes these server-side, which is more accurate and far cheaper than reading raw streams and doing the maths yourself.
3. Fetch the matching detail tool (`getRunningActivityDetail`, `getCyclingActivityDetail`, or `getSwimmingActivityDetail`) when you need what `analyzeActivityStream` does not cover: the device laps, or the opt-in durability and curve data (`with_dfa_alpha1` for the internal/HRV drift view, `with_power_curve` for the pace/power curve and within-session fade). Keep `with_time_series_metrics` off unless you must inspect specific raw samples. For deeper numbers on one device lap, chain back into `analyzeActivityStream` with a range built from that lap's `start_s`/`end_s`.
4. Keep the recent-activity list from step 1 (or fetch it) so the session can be compared to the user's other recent activities of the same sport.

What to analyze (adapt to the sport and to what the data actually contains):

- For runs: how did pace hold up relative to heart rate (if present) through the workout, and how does the session compare to other recent runs? Did it meet its intended purpose versus what was planned? Where relevant, check whether any effort was near a recent best, and how pace held up as the run wore on (fade over distance). Every run pace you receive outside the device laps is GAP (gradient adjusted pace: `gradient_adjusted_pace`, the `gap` stream, `gap_m_per_s`) — report it as GAP, never as the pace on the user's watch. Only each lap's `avg_pace_device` is the watch's raw pace, and the two differ by design on hilly terrain (typically GAP faster on climb-heavy runs, possibly slower on descent-dominant ones; point at the ascent/descent when the user notices a gap). A large mismatch on an essentially flat run is NOT explained by GAP — flag it as a possible data issue instead of dismissing it. Never re-derive distance or duration from GAP.
- For rides: look at power and heart rate (if present). Was the user pacing well or fading? Pull the power data to see whether any effort was near a recent best and how power held up after accumulated work. Compare against other recent rides and the session's intended purpose.
- For swims: how did pacing hold up (and heart rate, if present), how does it compare to recent swims, and did the session meet its intended purpose?
- If the user logs HRV, note how internal drift during the session compared to their usual durability trend.
- Note how the session was structured (long blocks versus scattered efforts).
- If weather data is present in the activity, factor in how conditions (temperature, humidity) compared to recent sessions.
- If the activity has device laps, break the effort down lap by lap: how consistent were the splits, and how was execution across any intervals?

Call out whatever stands out most about this particular session rather than working through a fixed checklist. Lead with the one or two most notable findings, then the supporting detail.
