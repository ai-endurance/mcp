---
name: replan
description: Weekly plan review and replan. Use when the user wants to review or adjust their upcoming training week, get back on track after missed workouts, reshuffle their plan, or asks how their training plan is going. Do not use for daily readiness questions (use checkin), single-activity analysis (use analyze-activity), or weather adjustments (use adjust-for-weather).
---

Review the user's plan progress and adjust the upcoming week to get them back on track.

1. Call the `getPlanProgress` tool to see what the user has completed versus what was planned.
2. Call the `getPlannedWorkouts` tool for the upcoming 7 days.
3. Review adherence: which workouts were completed, missed, or partially done, and what that means for the week ahead (missed key sessions, accumulated fatigue, too much crammed into remaining days).
4. Propose specific modifications for the next 7 days to get the user back on track. Be concrete: which workout to move, skip, or adjust, and why. Respect recovery: do not stack hard sessions on consecutive days to make up for missed volume.
5. After the user agrees to the proposal (or a subset of it), apply the changes with the tools: `changeWorkoutDate` to move a workout, `skipWorkout` to drop one, `changeWorkoutAdvice` to attach adjusted instructions, and `createRideRunWorkout`, `createSwimWorkout`, or `createStrengthOtherWorkout` to add a session. Confirm each destructive change (skips, deletions) before making it.
6. Summarize the final state of the week after the changes are applied.

Notes:

- This skill adjusts individual workouts. It does not rebuild the whole training plan from scratch; for a full plan re-optimization, point the user to the replan feature in the AI Endurance app.
- If progress is on track, say so and do not invent changes.
