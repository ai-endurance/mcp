---
name: checkin
description: Daily check-in and readiness review. Use when the user wants their daily check-in, morning check-in, asks how they should train today, whether they are recovered or ready for their workout, or whether to train given how they feel. Do not use for analyzing a completed workout (use analyze-activity), for restructuring the upcoming week (use replan), or for weather-based adjustments (use adjust-for-weather).
---

Run a daily readiness check-in and advise the user on today's training.

1. If the user has not already described how they feel, ask briefly: overall fatigue, muscle soreness, sleep quality, and anything notable (illness, stress, travel). Keep it to one short question, not a form.
2. Call the `getRecoveryModel` tool to get objective recovery data: HRV, resting heart rate, and recovery scores from their synced wearable.
3. Call the `getPlannedWorkouts` tool for today's date to see what training is planned.
4. Weigh the subjective answers against the objective recovery data and today's plan, then give a clear recommendation: do the workout as planned, adjust it (lower intensity, shorter duration), swap it to another day, or rest. Explain the reasoning in two or three sentences, referencing the specific signals that drove the call.
5. If the recommendation is an adjustment and the user agrees, apply it: use `changeWorkoutAdvice` to attach the adjusted guidance to today's workout, `changeWorkoutDate` to move it, or `skipWorkout` to skip it. Always confirm with the user before modifying their plan.

Notes:

- This check-in is advisory only. The user's answers here are not saved to their AI Endurance recovery model; the daily check-in form in the AI Endurance app is where check-in data is recorded. If the user wants their subjective data tracked over time, point them to the app's check-in.
- If `getRecoveryModel` returns no recent data (no wearable synced), base the advice on the subjective answers and today's plan alone, and say that objective recovery data was unavailable.
