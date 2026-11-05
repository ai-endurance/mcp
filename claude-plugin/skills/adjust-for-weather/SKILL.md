---
name: adjust-for-weather
description: Adapt the pacing of today's run to the weather forecast. Use when the user wants to adjust today's run for heat, humidity, cold, wind, or rain, or asks how to pace their planned run given the forecast. Do not use for general training readiness questions (use checkin) or for restructuring the training week (use replan).
---

Check the forecast and advise how to pace today's run for the conditions.

1. Establish location and timing. Ask the user where they will be running if their location is not already known from the conversation. Assume they will be training in the next 1-2 hours unless they say otherwise.
2. Look up the forecast for that window at their location using web search. Use the apparent (feels-like) temperature and humidity, not just the raw temperature. Wind and precipitation are secondary but worth noting when significant.
3. Call the `getPlannedWorkouts` tool for today to see what run is planned (target pace, duration, structure).
4. Make the call: state whether any pacing adjustment is needed to hit the workout's training goal. If conditions are typical for the user, say so plainly and leave the workout unchanged. Otherwise say what to change (pace, effort targets, or duration) so the session still delivers its intended training effect. In hot or humid conditions the usual adjustment is a slower pace at the same perceived effort, not a shorter workout.
5. State the time window and location you assumed, and ask if the user is planning a different time of day. If a later window has meaningfully better conditions for the session's goal, mention it.
6. If the user wants the adjustment recorded on the workout, apply it with the `changeWorkoutAdvice` tool so the adjusted pacing guidance shows up on their planned run.

Notes:

- If no run is planned today, say so and ask whether they want pacing advice for a different day's run or an unplanned run.
- This skill is about training pacing and workout execution, not a substitute for the user's own judgment about conditions.
