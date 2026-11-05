# AI Endurance plugin for Claude

AI Endurance builds adaptive training plans for runners, cyclists and triathletes. This plugin connects Claude to your AI Endurance account and adds four workflows that use your training plan, your completed activities and your recovery data.

## Skills

| Skill | What it does | Try asking |
|---|---|---|
| `checkin` | Weighs how you feel against your recovery data (HRV, resting heart rate) and today's planned workout, then recommends training as planned, adjusting, moving the session or resting. | "Run my daily check-in" |
| `analyze-activity` | Breaks down a completed run, ride or swim: pacing, power, heart rate, laps, and how the session compares to your recent training. Uses your most recent activity when you name none. | "How did my ride go yesterday?" |
| `replan` | Reviews what you completed against the plan and proposes concrete changes to the next 7 days, then applies the ones you agree to. | "I missed two workouts, help me fix my week" |
| `adjust-for-weather` | Looks up the forecast for your run and tells you how to pace it in heat, humidity, cold, wind or rain. Needs web search turned on in Claude. | "Adjust today's run for the weather" |

Claude asks before it changes anything in your plan.

## Connector

The plugin uses the AI Endurance MCP server at `https://aiendurance.com/mcp`, the same server as the [AI Endurance connector](https://claude.ai/directory/ai-endurance) in the Claude directory. If you already use that connector, the plugin works with it and adds the skills on top. The first time Claude calls a tool you sign in with your AI Endurance account (OAuth 2.0).

The full tool reference is in the [repository README](https://github.com/ai-endurance/mcp#available-tools-27).

## Requirements

- An AI Endurance account with an active subscription or free trial. Without one the tools answer with a notice that a subscription is required. Sign up at https://aiendurance.com.
- Synced training data (Garmin, Strava, Wahoo and others) for activity analysis, and a wearable that records HRV for the recovery data in the check-in.

## Data and privacy

The skills are plain instructions and run no code of their own. Your training data is read from and written to your AI Endurance account through the MCP server, and only when Claude calls a tool in your conversation. You can revoke access at any time by disconnecting the connector in Claude.

- Privacy policy: https://aiendurance.com/privacypolicy
- Terms of use: https://aiendurance.com/termsofuse

## Support

- Documentation: https://aiendurance.com/docs/mcp
- Email: info@aiendurance.com

## License

MIT, see [LICENSE](LICENSE). The license covers the files in this folder, not the AI Endurance service.
