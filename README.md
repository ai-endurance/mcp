# AI Endurance MCP Server

Connect your AI Endurance training platform to Claude and other AI assistants for conversational access to your training data, workouts, and performance analytics and to manage your training plan.

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

## Supported Platforms

<!--
### ChatGPT (beta mode for Apps & Connectors)

**Setup:**
1. Navigate to ChatGPT settings
2. Go to "Apps & Connectors"
3. Scroll down and open "Advanced Settings"
4. Enable "Developer Mode"
5. In "Apps & Connectors", select "Create"
6. Use the following configuration:

```yaml
Name: AI Endurance
MCP Server URL: https://aiendurance.com/mcp
Authentication Type: OAuth
```

7. Authorize with your AI Endurance credentials
8. Start asking questions about your training

**Example:**
```
You: "Show me my workouts for this week"
ChatGPT: [Lists your upcoming workouts with interactive widgets]
```
-->

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
- Access to Claude Pro or MCP-compatible client

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

## Available Tools (21)

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

Returns:
- Array of workouts with date, title, type, duration, zones, intervals
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

**`markWorkout`**
Mark a workout as completed or incomplete. Does not sync to external platforms (internal tracking only).

Parameters:
- `workoutId`: Database ID of workout
- `isCompleted`: Boolean (true = completed, false = incomplete)
- `title` (optional): Workout title for display

Returns:
- Success confirmation
- Completion status

**`changeWorkoutAdvice`**
Add or update coaching advice for a specific workout without modifying the workout structure.

Parameters:
- `workoutId`: Database ID of workout
- `advice`: Additional instructions or tips
- `title` (optional): Workout title for display

Returns:
- Success confirmation
- Updated advice text

**`createRideRunWorkout`**
Create custom structured workout for cycling or running with intervals, repeats, and zones.

Parameters:
- `dateStr`: Date in YYYY-MM-DD format
- `title`: Workout name
- `actType`: "Ride" or "Run"
- `stepsGeneral`: Array of step objects
- `isTaper` (optional): Boolean, marks as taper workout (default false)
- `advice` (optional): Coaching notes

Returns:
- Success confirmation
- Created workout ID

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
List recent cycling activities. Returns up to 40 most recent rides if no date range specified.

Parameters:
- `startDate` (optional): YYYY-MM-DD format
- `endDate` (optional): YYYY-MM-DD format

Returns:
- Array of cycling activities with summary metrics
- Activity name, date, duration, distance, power, heart rate, External Stress Score (ESS)

**`getRunningActivity`**
List recent running activities. Returns up to 40 most recent runs if no date range specified.

Parameters:
- `startDate` (optional): YYYY-MM-DD format
- `endDate` (optional): YYYY-MM-DD format

Returns:
- Array of running activities with summary metrics
- Activity name, date, duration, distance, pace, heart rate, running power

**`getSwimmingActivity`**
List recent swimming activities. Returns up to 40 most recent swims if no date range specified.

Parameters:
- `startDate` (optional): YYYY-MM-DD format
- `endDate` (optional): YYYY-MM-DD format

Returns:
- Array of swimming activities with summary metrics
- Activity name, date, duration, distance, pace, stroke rate

**`getCyclingActivityDetail`**
Detailed metrics for specific cycling activity including time-series data (power, heart rate, cadence, speed, elevation).

Parameters:
- `activityId`: Activity database ID (from getCyclingActivity)
- `resolution` (optional): Data resolution
  - `"low"`: ~200 points, ~5KB, ~1,250 tokens (default)
  - `"medium"`: ~500 points, ~12KB, ~3,000 tokens
  - `"high"`: ~1000 points, ~25KB, ~6,250 tokens
  - `"full"`: All data points (18k-125k tokens - use sparingly!)

Returns:
- Complete activity metadata
- Time-series metrics: power, heart rate, cadence, speed, elevation, latitude, longitude
- Zone distributions
- Best efforts (peak power across durations)

**`getRunningActivityDetail`**
Detailed metrics for specific running activity including time-series data (pace, heart rate, power, elevation, cadence).

Parameters:
- `activityId`: Activity database ID (from getRunningActivity)
- `resolution` (optional): Data resolution (same as cycling)

Returns:
- Complete activity metadata
- Time-series metrics: pace, heart rate, running power, elevation, cadence, latitude, longitude
- Zone distributions
- Best efforts (peak pace/power across durations)

**`getSwimmingActivityDetail`**
Detailed metrics for specific swimming activity including time-series data (pace, stroke rate, distance per stroke).

Parameters:
- `activityId`: Activity database ID (from getSwimmingActivity)
- `resolution` (optional): Data resolution (same as cycling)

Returns:
- Complete activity metadata
- Time-series metrics: pace, stroke rate, distance per stroke, pool length
- Lap-by-lap breakdown
- Stroke analysis

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

Returns:
- Time-series data showing recovery trends (past 30 days)
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
- Create or modify data exclusions
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
<!-- - **ChatGPT** (web interface with Apps & Connectors in Developer Mode) -->
- **Claude.ai** (web interface)
- **Claude Desktop** (macOS)

### Compatible (not officially tested)
- Any MCP 2025-06-18 compliant client using Streamable HTTP or SSE transport
- Cursor, Continue, Cline (developer tools)
- Custom MCP client implementations

## Changelog

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
