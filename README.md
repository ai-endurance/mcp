# AI Endurance MCP Server

Connect your AI Endurance training platform to ChatGPT, Claude, and other AI assistants for conversational access to your training data, workouts, and performance analytics and to manage your training plan.

## Overview

The AI Endurance MCP server enables AI assistants to access your training plan, activity history, performance predictions, recovery metrics, and training zones through natural conversation. You can view, modify, and create structured workouts for cycling, running, and swimming, analyze detailed activity data including power curves and pace trends, track your recovery using HRV and resting heart rate, and get machine learning-based race time predictions.

## Features

- **Training Plan Management** - View, modify, and create workouts with structured intervals
- **Activity Analysis** - Access detailed metrics from cycling, running, and swimming activities
- **Performance Predictions** - ML-based race time predictions and fitness forecasting
- **Recovery Tracking** - Monitor HRV, resting heart rate, and readiness to train
- **Zone Management** - Update and view training zones (pace, power)
- **Workout Scheduling** - Move workouts, adjust availability, track plan progress
- **Race Goals** - Manage primary and secondary race objectives
- **Activity Flags** - Correct indoor/virtual/erg detection and exclude bad-sensor activities from analysis
- **Durability** - See how power or pace held up as work accumulated within a session, and how that compares to your own trend
- **Computed Activity Analytics** - Server-side normalized power, intensity factor, time-in-zone, pacing/fade and split tables for any activity, without reading raw streams
- **Other Sports** - List strength, ski, yoga, hike and other non-run/ride/swim activities

## Supported Platforms

### ChatGPT

AI Endurance is available in the ChatGPT plugin directory: https://chatgpt.com/plugins/plugin_asdk_app_69456fbb59d081918bcb148a12380f92

**Setup:**
1. Open the plugin directory in ChatGPT and search for "AI Endurance" (or use the link above)
2. Select "Connect"
3. Authorize with your AI Endurance account
4. Start asking questions about your training

ChatGPT additionally renders interactive widgets for most tools, so workouts, activities, recovery, and predictions come back as rich cards rather than plain text.

**Example:**
```
You: "Show me my workouts for this week"
ChatGPT: [Lists your upcoming workouts with interactive widgets]
```

### Claude.ai

**Setup:**
1. Navigate to Claude.ai settings
2. Go to "Connectors"
3. Select "Add custom connector"
4. Use the following configuration:

```yaml
Name: AI Endurance
Remote MCP Server URL: https://aiendurance.com/mcp
```

5. Click "Add"
6. Authorize with your AI Endurance account
7. Start asking questions about your training

**Example:**
```
You: "How was my ride yesterday?"
Claude: [Displays power distribution, External Stress Score, duration, and zone breakdown]
```

### Other MCP-Compatible Clients

Any MCP 2025-06-18 compliant client can connect using:

**Streamable HTTP Configuration (Recommended):**
```json
{
  "url": "https://aiendurance.com/mcp",
  "transport": {
    "type": "http"
  },
  "auth": {
    "type": "oauth",
    "authorizationUrl": "https://aiendurance.com/authorize/",
    "tokenUrl": "https://aiendurance.com/api/o/token/",
    "scopes": ["read", "write"]
  }
}
```

**SSE Transport Configuration (Legacy):**
```json
{
  "url": "https://aiendurance.com/mcp",
  "transport": {
    "type": "sse"
  },
  "auth": {
    "type": "oauth",
    "authorizationUrl": "https://aiendurance.com/authorize/",
    "tokenUrl": "https://aiendurance.com/api/o/token/",
    "scopes": ["read", "write"]
  }
}
```

**Compatible Clients:**
- Claude Desktop (macOS, Windows)
- Cursor (code editor with AI)
- Continue (VS Code extension)
- Cline
- Any custom MCP client implementation

## Prerequisites

- AI Endurance account (sign up at https://aiendurance.com)
- Active AI Endurance subscription or free trial
- ChatGPT, Claude, or any other MCP-compatible client

## Example Conversations

### Training Plan Analysis

```
You: "Show me my workouts for this week"
AI: [Lists 6 workouts with dates, types, durations, and training zones]

You: "What's my long run this weekend?"
AI: [Shows Saturday's 90-minute endurance run with pace zones]

You: "Move tomorrow's threshold workout to Friday"
AI: [Reschedules workout and confirms sync to Garmin/TrainingPeaks]

You: "Am I training enough at threshold?"
AI: [Analyzes plan progress showing actual vs prescribed threshold time]
```

### Activity Deep Dives

```
You: "How was my ride yesterday?"
AI: [Displays power distribution, normalized power, stress scores, duration, and zone breakdown]

You: "What was my average pace on runs this month?"
AI: [Analyzes all January runs and calculates average pace, weekly volume]

You: "Show me the power curve from my last cycling activity"
AI: [Provides detailed time-series power data with peak power efforts]

You: "Compare my last 3 long runs"
AI: [Pulls detailed metrics and compares pace, heart rate, duration trends]

You: "Yesterday's ride was on Zwift, not outdoors"
AI: [Marks the activity indoor and virtual, and updates the stored weather]

You: "My HR strap was dead on this run - don't use its heart rate"
AI: [Flags the heart rate data as unreliable and rebuilds the HRV aggregates]
```

### Recovery & Fitness

```
You: "Am I recovered enough for today's hard workout?"
AI: [Shows recovery score, HRV trend, resting HR, and training recommendation]

You: "What's my predicted half marathon time based on current fitness?"
AI: [Displays ML-based prediction with confidence intervals and improvement trajectory]

You: "How well am I following my training plan?"
AI: [Shows plan adherence by zone with actual vs prescribed training volume]

You: "What does my HRV trend say about my fitness?"
AI: [Analyzes recovery model data and provides insights on adaptation]
```

### Custom Workout Creation

```
You: "Create a threshold run for tomorrow: 15min warmup, 3x8min at threshold with 2min recovery, 10min cooldown"
AI: [Creates structured workout with proper zones, syncs to Garmin/TrainingPeaks/Zwift]

You: "Build me a 60min tempo ride at 85% FTP for Sunday"
AI: [Creates power-based cycling workout with appropriate structure]

You: "Design a swim workout: 200m warmup, 5x100m at threshold pace with 20sec rest, 200m cooldown"
AI: [Creates detailed swim workout with sets, strokes, and pace zones]
```

### Race Planning

```
You: "What are my upcoming race goals?"
AI: [Lists primary and secondary races with dates and target times]

You: "Based on my training, how realistic is my marathon goal?"
AI: [Analyzes predictions, current training load, and provides assessment]

You: "Show me my fitness trend over the last 8 weeks"
AI: [Displays prediction model history showing fitness progression]
```

## Available Tools (27)

### Profile & Settings

**`getUser`**
View your profile including training zones, user type (Runner/Cyclist/Triathlete), units (Metric/Imperial), and preferences.

Returns:
- Training zones (cycling power, running pace/power)
- Heart rate thresholds
- Physical metrics (weight, height, birth year)
- User preferences

**`setZones`**
Update training zones for cycling (power) or running (pace/power). Automatically manages both pace and power zones for runners using running power meters.

Parameters:
- `actType`: "Run" or "Ride"
- `zones`: Object with zone upper bounds
  - `Endurance`: Upper limit (e.g., "5:31 /km" or "200 W")
  - `Tempo`: Upper limit
  - `Threshold`: Upper limit
  - `VO2Max`: Upper limit

Note: Must include unit in each value. For running, use pace format "mm:ss /km" or "mm:ss /mi", or power format "XXX W". For cycling, use power format "XXX W".

**`getAvailability`**
View weekly training hours and daily availability schedule for each activity type.

Returns:
- Weekly hours breakdown (total and by sport for triathletes)
- Daily schedule with available training times

### Workout Management

**`getPlannedWorkouts`**
Retrieve planned workouts for a date range (default: next 14 days).

Parameters:
- `startDate` (optional): Start date in YYYY-MM-DD format (defaults to today)
- `endDate` (optional): End date in YYYY-MM-DD format (defaults to today + 14 days)
- `summaryMode` (optional): Boolean - if true, returns lightweight overview with minimal fields, no 35-day cap
- `fullDetails` (optional): Boolean - if true, includes the machine-readable step structure (`steps_general`, `swim_sections`, zone distribution, compliance data) and untruncated swim intervals

Returns:
- Array of workouts with date, title, type, duration, and human-readable warmup/intervals/cooldown descriptions
- `has_steps_general` per workout, indicating whether a machine-readable structure exists (retrieve it with `fullDetails`)
- Workout density metrics (workouts per week)
- Applied date range

**`changeWorkoutDate`**
Move a workout to a different date. Updates workout schedule and syncs with all connected platforms (Garmin, TrainingPeaks, Zwift, etc.).

Parameters:
- `workoutId`: Database ID of workout
- `newDate`: New date in YYYY-MM-DD format
- `title` (optional): Workout title for display purposes

Returns:
- Success confirmation
- Old and new dates

**`skipWorkout`**
Remove a workout from the training plan. Marks workout as skipped and syncs deletion to connected platforms.

Parameters:
- `workoutId`: Database ID of workout
- `title` (optional): Workout title for display

Returns:
- Success confirmation
- Workout details

**`changeWorkoutAdvice`**
Add or update coaching advice for a specific workout without modifying the workout structure.

Parameters:
- `workoutId`: Database ID of workout
- `advice`: Additional instructions or tips
- `title` (optional): Workout title for display

Returns:
- Success confirmation
- Updated advice text

**`changeWorkoutIntensity`**
Change the intensity (load) of an existing planned ride or run workout in place. The workout's intensity zone is preserved - the step durations are recomputed at the new load.

Parameters:
- `workoutId`: Database ID of workout (the `workout_id` field of a `getPlannedWorkouts` result)
- `ess`: New training stress score (required if `intensityTime` not provided)
- `intensityTime`: New time at intensity in seconds (required if `ess` not provided)
- `repeats` (optional): New number of repeats at the intensity
- `title` (optional): Workout title for display

Returns:
- Success confirmation with the updated title, date and training stress

Note: only works on workouts with a scalable step structure (algorithm-generated plan workouts and workouts from `createRideRunWorkoutByIntensity` both qualify - `has_steps_general` is false on `getPlannedWorkouts`). On a structured workout it fails cleanly with `WORKOUT_HAS_NO_STEPS`; skip the workout and recreate it with `createRideRunWorkout` or `createRideRunWorkoutByIntensity` instead.

**`createRideRunWorkout`**
Create custom structured workout for cycling or running with intervals, repeats, and zones.

Parameters:
- `dateStr`: Date in YYYY-MM-DD format
- `title`: Workout name
- `actType`: "Ride" or "Run"
- `stepsGeneral`: Array of step objects (zone-based targets)
- `isTaper` (optional): Boolean, marks as taper workout (default false)
- `advice` (optional): Coaching notes

Returns:
- Success confirmation
- Created workout ID

**`createRideRunWorkoutAdvanced`**
Create a ride or run workout with precise numeric power or pace targets - ramp tests, FTP tests, over/under intervals, exact-watt or exact-pace sessions. For simple zone-based workouts use `createRideRunWorkout` instead.

Parameters:
- Same as `createRideRunWorkout`, except each `stepsGeneral` step additionally supports `targetType` (POWER for watts, SPEED for m/s pace, HEART_RATE for bpm, etc.), a numeric `targetValue`, and explicit `targetValueLow`/`targetValueHigh` bounds. Without explicit bounds the backend derives a +/-5% range around `targetValue`.

Returns:
- Success confirmation
- Created workout ID

**`createRideRunWorkoutByIntensity`**
Create a simple ride or run workout from one intensity zone plus a target load - no step structure needed. For structured workouts with custom warmup/interval/cooldown steps use `createRideRunWorkout` instead.

Parameters:
- `dateStr`: Date in YYYY-MM-DD format
- `actType`: "Ride" or "Run" (must match the user's sport: Runner users only Run, Cyclist users only Ride, Triathlete users both)
- `intensityType`: "Endurance", "Tempo", "Threshold", "VO2Max" or "Anaerobic"
- `ess`: Target training stress score (required if `intensityTime` not provided; more than about 100 is a hard workout)
- `intensityTime`: Target time at intensity in seconds (required if `ess` not provided)
- `repeats` (optional): Number of repeats at the intensity, for Tempo and above
- `isTaper` (optional): Boolean, marks as taper workout (default false)

Returns:
- Success confirmation
- Created workout ID and title

Note: if a workout with the same date, sport and load already exists, that existing workout is returned instead of a duplicate.

**`createSwimWorkout`**
Create custom swim workout with structured sections (warmup, preparation, main, cooldown), sets, intervals, strokes, and equipment.

Parameters:
- `dateStr`: Date in YYYY-MM-DD format
- `title`: Workout name
- `swimSections`: Array of swim section objects
- `isTaper` (optional): Boolean, marks as taper workout (default false)
- `advice` (optional): Coaching notes

Returns:
- Success confirmation
- Created workout ID

**`createStrengthOtherWorkout`**
Create custom strength or other non-swim/bike/run workout, e.g. cross-country skiing, yoga, hiking.

Parameters:
- `dateStr`: Date in YYYY-MM-DD format
- `title`: Workout name
- `strengthOtherText`: The workout description/instructions in free-form text
- `isTaper` (optional): Boolean, marks as taper workout (default false)

Returns:
- Success confirmation
- Created workout ID

### Activity History

**`getCyclingActivity`**
List recent cycling activities. Returns the 20 most recent rides if no date range specified, up to 40 with a date range.

Parameters:
- `startDate` (optional): YYYY-MM-DD format
- `endDate` (optional): YYYY-MM-DD format
- `with_dfa_alpha1` (optional): Boolean - if true, includes the DFA alpha 1 and aerobic/anaerobic threshold fields per activity

Returns:
- Array of cycling activities with summary metrics
- `id`: the activity id - pass it as `activityId` to `getCyclingActivityDetail` or `setActivityFlags`
- Activity name, date, duration, distance, power, heart rate, External Stress Score (ESS), weather

**`getRunningActivity`**
List recent running activities. Returns the 20 most recent runs if no date range specified, up to 40 with a date range.

Parameters:
- `startDate` (optional): YYYY-MM-DD format
- `endDate` (optional): YYYY-MM-DD format
- `with_dfa_alpha1` (optional): Boolean - if true, includes the DFA alpha 1 and aerobic/anaerobic threshold fields per activity

Returns:
- Array of running activities with summary metrics
- `id`: the activity id - pass it as `activityId` to `getRunningActivityDetail` or `setActivityFlags`
- Activity name, date, duration, distance, pace, heart rate, running power, weather

**`getSwimmingActivity`**
List recent swimming activities. Returns up to 40 most recent swims if no date range specified.

Parameters:
- `startDate` (optional): YYYY-MM-DD format
- `endDate` (optional): YYYY-MM-DD format

Returns:
- Array of swimming activities with summary metrics
- `id`: the activity id - pass it as `activityId` to `getSwimmingActivityDetail`
- Activity name, date, duration, distance, pace, stroke rate

**`getCyclingActivityDetail`**
Detailed data for one cycling activity. The default response is deliberately light; the durability, power-curve and raw sample data are each opt-in.

Parameters:
- `activityId`: the activity id (the `id` field of a `getCyclingActivity` result)
- `with_dfa_alpha1` (optional): Boolean - adds the DFA alpha 1 threshold values and `durability_drift`
- `with_power_curve` (optional): Boolean - adds the peak power curve, % of recent best, effort structure and `within_session_durability`
- `with_time_series_metrics` (optional): Boolean - adds the raw per-sample arrays. Default false
- `resolution` (optional): sampling for the raw arrays, so it has no effect unless `with_time_series_metrics` is true
  - `"low"`: ~200 points, ~5KB, ~1,250 tokens (default)
  - `"medium"`: ~500 points, ~12KB, ~3,000 tokens
  - `"high"`: ~1000 points, ~25KB, ~6,250 tokens
  - `"full"`: All data points (18k-125k tokens - use sparingly!)

Returns by default:
- `id` and the complete activity metadata (date, duration, distance, average power/HR, stress scores, weather, the activity flags)
- `laps`: the device laps the head unit recorded, with per-lap power, HR, cadence and respiration

With `with_dfa_alpha1`:
- The aerobic/anaerobic threshold values, the a1 scalars, and each lap's average a1
- `durability_drift`: how this ride's internal drift (heart rate, DFA a1, respiration frequency) sat against your **own** fitted ~6-week trend at matched work - mean residual, position versus the confidence band, the trend's %-loss at the anchors, and the number of rides behind the trend. Needs clean R-R, so it is absent on rides without it

With `with_power_curve`:
- `power_curve` and `pct_of_recent_best` (percent of your recent best at each duration)
- `effort_structure`: time spent by intensity band and bout length
- `within_session_durability`: how far sustained power fell off as work accumulated within the ride, along the ride's own kJ axis. Needs no HRV, so it is available on essentially any ride with power

With `with_time_series_metrics`:
- The raw per-sample arrays: power, heart rate, cadence, altitude, respiration frequency (plus the a1 channels when `with_dfa_alpha1` is also set), sampled to `resolution`

**`getRunningActivityDetail`**
Detailed data for one running activity. Same opt-in structure as the cycling detail tool.

Parameters:
- `activityId`: the activity id (the `id` field of a `getRunningActivity` result)
- `with_dfa_alpha1` (optional): Boolean - adds the DFA alpha 1 threshold values and `durability_drift`
- `with_power_curve` (optional): Boolean - adds the peak GAP-pace and running-power curves, % of recent best, effort structure and `within_session_durability`
- `with_time_series_metrics` (optional): Boolean - adds the raw per-sample arrays. Default false
- `resolution` (optional): sampling for the raw arrays (same as cycling), so it has no effect unless `with_time_series_metrics` is true

Returns by default:
- `id` and the complete activity metadata (date, duration, distance, average pace/power/HR, stress scores, weather, the activity flags)
- `laps`: the device laps the watch recorded, with per-lap pace, power, HR, cadence and respiration

With `with_dfa_alpha1`:
- The aerobic/anaerobic threshold values, the a1 scalars, and each lap's average a1
- `durability_drift`: this run's internal drift (heart rate, DFA a1, respiration frequency) against your **own** fitted ~6-week trend at matched work. Needs clean R-R, so it is absent on runs without it

With `with_power_curve`:
- `pace_curve`, `running_power_curve` and `pct_of_recent_best`
- `effort_structure`: time spent by intensity band and bout length
- `within_session_durability`, split by channel (`gap` for GAP pace, `power` for running power): how far sustained pace or power fell off as distance accumulated within the run, along its own GAP-km axis. Needs no HRV, so it is available on essentially any run

With `with_time_series_metrics`:
- The raw per-sample arrays: pace, heart rate, running power, altitude, cadence, respiration frequency (plus the a1 channels when `with_dfa_alpha1` is also set), sampled to `resolution`

**`getSwimmingActivityDetail`**
Detailed metrics for specific swimming activity including time-series data (pace, stroke rate, distance per stroke).

Parameters:
- `activityId`: the activity id (the `id` field of a `getSwimmingActivity` result)
- `with_time_series_metrics` (optional): Boolean - adds the raw per-sample arrays. Default false
- `resolution` (optional): sampling for the raw arrays (same as cycling), so it has no effect unless `with_time_series_metrics` is true

Returns:
- Complete activity metadata
- Time-series metrics: pace, stroke rate, distance per stroke, pool length
- Lap-by-lap breakdown
- Stroke analysis

**`analyzeActivityStream`**
Computes quantitative analytics for one activity server-side and returns a compact summary. Prefer this over the detail tools whenever you want numbers - normalized power, intensity factor, variability, time-in-zone, pacing/fade (first vs second half), or channel extremes (avg/max/min power, heart rate, cadence, pace). Not for durability, DFA alpha 1 thresholds, or the mean-max curve - those live behind the detail tools' opt-in flags.

Parameters:
- `activityId`: the activity id (the `id` field of an activity list result)
- `activityType`: "Ride", "Run" or "Swim"
- `segments` (optional): "auto" (default) adds a small table of equal time-window splits (avg power/speed + HR per window); "none" skips it. These are computed windows, not the device laps.
- `range` (optional): `{"type": "time_seconds", "from": seconds, "to": seconds}` restricts the whole analysis to a time window - e.g. the first 30 minutes, or one device lap via the detail tools' `start_s`/`end_s`

Returns (blocks omitted when the activity lacks the data):
- Overview: moving/elapsed time, distance, elevation gain
- `power`: avg/max/min, normalized power, variability index, intensity factor
- `heart_rate`, `cadence`, `pace_m_per_s`
- `pacing`: first vs second half averages and `fade_pct` (positive = second half lower power / slower; terrain-naive, so check the per-half ascent/descent before calling a fade physiological)
- `time_in_zone` and `segments`

**`getOtherActivity`**
List activities from any sport outside running, cycling, and swimming - strength training, cross-country skiing, yoga, hiking, walking. Returns the 20 most recent if no date range specified, up to 40 with a date range.

Parameters:
- `startDate` (optional): YYYY-MM-DD format
- `endDate` (optional): YYYY-MM-DD format

Returns:
- Array of activities with name, type, date, duration, average heart rate, stress scores, elevation gain, distance, calories

Note: other activities are duration-only. There is no time-series/stream data and no detail tool for them, so do not expect power, pace, HRV, or per-second metrics.

### Activity Flags

**`setActivityFlags`**
Set per-activity flags on a cycling or running activity: indoor, virtual, erg mode, and read-time analysis exclusions. Use when an activity was misdetected (an indoor ride treated as outdoor) or when bad sensor data should be kept out of the analyses. Only the flags you pass change; the others stay untouched.

Parameters:
- `activityId`: the activity id (the `id` field of a `getCyclingActivity` / `getRunningActivity` result)
- `sport`: "cycling" or "running"
- `isIndoor` (optional): Activity was performed indoors (trainer/treadmill/virtual). Also swaps the stored weather to the indoor marker, or re-fetches outdoor weather when flipped back to outdoor.
- `isVirtual` (optional): Virtual ride/run (Zwift, Rouvy, etc.). Implies indoor.
- `isErgMode` (optional): Recorded in erg mode (the trainer controls power)
- `excludeFromCurves` (optional): Exclude from aggregate power/pace-duration curves and recent-best comparisons (e.g. power meter malfunction)
- `excludeFromModel` (optional): Exclude from digital twin (GRU) model training data
- `excludeFromDurability` (optional): Exclude from durability curve aggregation
- `excludeHrData` (optional): Heart rate data is unreliable (e.g. strap failure) - excludes the activity from HRV/alpha 1 aggregation and from model training while keeping the power/pace analyses

Returns:
- Success confirmation and a human-readable summary of what changed
- `flags`: current values of all seven flags after the update
- `retrain_queued`: whether the change queued a digital twin retrain (`excludeFromModel` and `excludeHrData` do; `excludeHrData` additionally rebuilds the stored HRV aggregates)

Notes: flags you set by hand are pinned, so later automatic detection will not overwrite them. The flag values are also returned on every activity in the `getCyclingActivity` / `getRunningActivity` list and detail results.

### Analytics & Insights

**`getRaceGoalEvent`**
View primary and secondary race goal events with performance predictions and priorities.

Returns:
- Primary race goal (name, date, distance, priority, predicted time)
- Secondary race goals (if configured)
- Days until each race
- Target finish times

**`getPrediction`**
ML-based performance predictions including future forecasts, historical data, and model validation metrics.

Returns:
- Future predictions (next 12 weeks of fitness trajectory)
- Historical predictions (actual vs predicted comparison)
- Model validation scores
- Confidence intervals
- Training impact on predictions

**`getRecoveryModel`**
Recovery model data including:
- Cardio recovery score
- DFA alpha 1 (cardiac autonomic metric from HRV analysis)
- rMSSD (heart rate variability - parasympathetic activity)
- Resting heart rate trends
- External stress score
- Orthopedic recovery (joint/muscle recovery for cycling, running, swimming)

Parameters:
- `days_back` (optional): How many days of daily recovery data to return, 1-90 (defaults to 14)

Returns:
- Time-series data showing recovery trends (past 14 days by default)
- Current recovery status
- Recovery drivers (what's limiting recovery today)
- Activity-specific orthopedic recovery

**`getPlanProgress`**
Training plan progress showing adherence to prescribed training zones.

Returns:
- Match percentage (overall plan adherence)
- Zone-by-zone breakdown:
  - Endurance: actual hours vs prescribed hours
  - Tempo: actual vs prescribed
  - Threshold: actual vs prescribed
  - VO2Max: actual vs prescribed
  - Anaerobic: actual vs prescribed
- For triathletes: separate progress for Ride, Run, Swim

**`getNutritionModel`**
Retrieves the user nutrition model with daily calorie and macronutrient requirements (protein, fat, carbohydrates) including lower and upper bounds.

Returns:
- Daily calorie and macronutrient requirements for 6 days (1 past day + today + 5 future days)
- Protein requirements (lower and upper bounds in grams)
- Fat requirements (lower and upper bounds in grams)
- Carbohydrate requirements (lower and upper bounds in grams)
- Based on planned workouts and user physiology

## Authentication & Security

### OAuth 2.0 Flow

1. AI assistant initiates OAuth flow
2. User redirected to AI Endurance authorization page
3. User signs in with AI Endurance credentials
4. User grants "read" scope access
5. AI Endurance returns authorization code
6. AI assistant exchanges code for access token
7. All API requests authenticated via Bearer token

### Scopes

- **read**: View training data, workouts, activities, zones, predictions, and recovery metrics
- **write**: Create, modify, and delete workouts; update training zones; manage workout schedule

### Data Access

The MCP server has access to:
- User profile and preferences
- Training zones (view and modify)
- Planned workouts (view, modify schedule, create new)
- Activity history (cycling, running, swimming)
- Performance predictions
- Recovery metrics
- Race goals

The MCP server **cannot**:
- Start training plan generation
- Create or modify date-range data exclusions (per-activity flags are settable with `setActivityFlags`)
- Alter your connections to third-party platforms (Garmin, Strava, etc.)
- Delete your account
- Modify account billing settings
- Access payment information
- Delete historical activities (can only skip future workouts)

### Revocation

Disconnect access anytime in your mcp client.

## Technical Specifications

- **Protocol Version**: MCP 2025-06-18
- **Transport**: Streamable HTTP (preferred) or SSE (legacy)
- **Authentication**: OAuth 2.0
- **Message Format**: JSON-RPC 2.0
- **Base URL**: https://aiendurance.com/mcp
- **Messages Endpoint**: https://aiendurance.com/mcp/messages
- **Manifest**: https://aiendurance.com/.well-known/ai-plugin.json

### Rate Limits

No explicit rate limits currently enforced. Standard API usage guidelines apply - avoid excessive requests in short time periods.

### Error Handling

Errors returned in MCP-compliant format:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "content": [{
      "type": "text",
      "text": "Error message here"
    }],
    "isError": true
  }
}
```

Common error codes:
- **401**: Authentication required or token expired
- **403**: Insufficient permissions
- **404**: Workout/activity not found
- **422**: Validation error (invalid parameters)
- **500**: Internal server error

## Support & Resources

- **Documentation**: https://aiendurance.com/docs/mcp
- **Support Email**: info@aiendurance.com
- **Main Website**: https://aiendurance.com
- **Terms of Use**: https://aiendurance.com/termsofuse
- **Privacy Policy**: https://aiendurance.com/privacypolicy

## Platform Compatibility

### Tested & Working
- **ChatGPT** (web, iOS, Android - from the plugin directory, with interactive widgets)
- **Claude.ai** (web interface)
- **Claude Desktop** (macOS)

### Compatible (not officially tested)
- Any MCP 2025-06-18 compliant client using Streamable HTTP or SSE transport
- Cursor, Continue, Cline (developer tools)
- Custom MCP client implementations

## Changelog

### Version 1.2.0 (2026-08-25)

**Added:**
- `analyzeActivityStream` tool: server-side computed analytics for one activity - normalized power, intensity factor, variability, time-in-zone, first-vs-second-half pacing/fade, channel extremes, and optional equal time-window splits, with an optional time range (e.g. one device lap). This is the recommended path for quantitative questions; the detail tools' raw arrays stay off by default.
- `getOtherActivity` tool: lists activities from any sport outside running, cycling, and swimming (strength, ski, yoga, hike, ...). Duration-only - no stream data and no detail tool.
- `createRideRunWorkoutByIntensity` tool: creates a simple ride or run workout from one intensity zone plus a target load (`ess` and/or `intensityTime`), without authoring a step list.
- `changeWorkoutIntensity` tool: rescales an existing planned ride or run workout in place to a new `ess` and/or `intensityTime`, preserving its intensity zone. Structured (steps_general) workouts fail cleanly with `WORKOUT_HAS_NO_STEPS` and should be skipped and recreated instead.

This brings the MCP tool surface to parity with the AI Endurance chatbot's backend tooling.

### Version 1.1.0 (2026-08-25)

**Added:**
- Every activity in `getCyclingActivity`, `getRunningActivity` and `getSwimmingActivity` now carries its `id`. Pass it as `activityId` to the matching detail tool or to `setActivityFlags`. Previously no response exposed an id, so the detail tools and `setActivityFlags` could not be called from a list result.
- `with_dfa_alpha1` on `getCyclingActivityDetail` and `getRunningActivityDetail`: the DFA alpha 1 threshold values plus `durability_drift` - how that session's internal drift (heart rate, DFA a1, respiration frequency) sat against your own fitted ~6-week trend at matched work. Needs clean R-R data.
- `with_power_curve` on the same two tools: the peak power/pace curve, percent of your recent best, the effort-structure summary, and `within_session_durability` - how far sustained power or pace fell off as work accumulated within the session, along its own kJ or GAP-km axis. Needs no HRV, so it is available on essentially every ride and run.
- `with_time_series_metrics` on all three detail tools: returns the raw per-sample arrays.

**Changed:**
- The detail tools no longer return the raw per-sample `time_series_metrics` arrays by default - set `with_time_series_metrics` to get them. The derived objects above answer pacing, fade and durability questions without them.
- The DFA alpha 1 threshold values on the detail tools now require `with_dfa_alpha1`, matching how the summary tools have gated them since 1.0.5.
- `resolution` only affects the raw arrays, so it is a no-op unless `with_time_series_metrics` is set.

Nothing was removed: both changes are opt-in, but a client that parsed the raw arrays or the a1 values from a detail response must now pass the corresponding flag.

**Fixed:**
- A detail tool called without an `activityId` returned a generic "Tool execution failed" error instead of an empty result.

### Version 1.0.6 (2026-08-20)

**Added:**
- `setActivityFlags` tool: sets the per-activity flags on a cycling or running activity - indoor, virtual, erg mode, and the read-time analysis exclusions (power/pace curves, digital twin model training, durability, unreliable heart rate data). Manually set flags are pinned against later automatic detection, setting `isIndoor` keeps the stored activity weather consistent, and the exclusions that change the training data queue a digital twin retrain (plus an HRV aggregate rebuild for `excludeHrData`).
- The flag fields are now returned on every activity in the `getCyclingActivity`, `getRunningActivity`, and the corresponding detail results.

### Version 1.0.5 (2026-08-06)

**Added:**
- `createRideRunWorkoutAdvanced` tool: creates ride/run workouts with precise numeric power or pace targets (ramp tests, FTP tests, over/unders, exact-watt or exact-pace sessions).
- `fullDetails` parameter on `getPlannedWorkouts`: returns the machine-readable step structure and untruncated swim intervals.
- `with_dfa_alpha1` parameter on `getCyclingActivity` and `getRunningActivity`: returns the DFA alpha 1 and threshold fields.
- `days_back` parameter on `getRecoveryModel`: widens the returned window up to 90 days.
- Weather at the activity start in the cycling and running activity summaries.

**Changed:**
- Leaner default responses so tool results stay small: `getPlannedWorkouts` returns human-readable workout descriptions plus a `has_steps_general` flag instead of the full step structure; `getCyclingActivity` and `getRunningActivity` return 20 activities without a date range (40 with one) and omit the DFA alpha 1 fields; `getRecoveryModel` returns the last 14 days instead of the full history. Each is restored by the corresponding parameter above.

### Version 1.0.4 (2026-03-23)

**Removed:**
- `markWorkout` tool: This tool no longer exists.

### Version 1.0.3 (2026-01-30)

**Changed:**
- Upgraded to MCP protocol version 2025-06-18
- Added Streamable HTTP transport support (preferred for new clients)
- SSE transport maintained for backwards compatibility
- Added `MCP-Protocol-Version` and `MCP-Session-Id` response headers

### Version 1.0.2 (2025-01-20)

**Added:**
- `getNutritionModel` tool: Retrieves daily calorie and macronutrient requirements (protein, fat, carbohydrates) with lower/upper bounds for 6 days (1 past + today + 5 future) based on planned workouts and user physiology.
- `training_plan_generation_system_prompt` added to `getUser` tool output for LLM context when generating training recommendations.

### Version 1.0.1 (2025-12-03)
- **createStrengthOtherWorkout tool**: new tool to create strength and other workouts (e.g. cross country skiing, yoga, hiking)

### Version 1.0.0 (2025-11-21)

**Initial Release**

- **MCP Protocol**: Implemented MCP 2025-03-26 specification with SSE transport
- **OAuth 2.0 Authentication**: Full OAuth flow with dynamic client registration (RFC 7591)
- **20 Tools**: Complete training management toolkit
  - Profile & Settings: `getUser`, `setZones`, `getAvailability`
  - Workout Management: `getPlannedWorkouts`, `changeWorkoutDate`, `skipWorkout`, `markWorkout`, `changeWorkoutAdvice`, `createRideRunWorkout`, `createSwimWorkout`
  - Activity History: `getCyclingActivity`, `getRunningActivity`, `getSwimmingActivity`, `getCyclingActivityDetail`, `getRunningActivityDetail`, `getSwimmingActivityDetail`
  - Analytics & Insights: `getRaceGoalEvent`, `getPrediction`, `getRecoveryModel`, `getPlanProgress`
- **20 Resources**: OpenAI Apps SDK UI components for rich ChatGPT widgets
- **5 Prompts**: Conversation templates for common training workflows
  - Training Plan Analysis
  - Activity Analysis
  - Recovery Check
  - Custom Workout Creation
  - Race Planning
- **Multi-Sport Support**: Cycling, running, swimming, and triathlon
- **Platform Support**: Claude.ai, Claude Desktop (macOS)
- **Documentation**: Comprehensive API documentation at https://github.com/ai-endurance/mcp

**Built by AI Endurance** - AI-powered data-driven training for runners, cyclists, and triathletes.
