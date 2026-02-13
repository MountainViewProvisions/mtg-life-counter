# MTG Life Counter - Complete Plugin Documentation

**Version:** 4.3.1  
**Author:** Mountain View Provisions
**License:** Custom  
**WordPress Compatibility:** 5.8+  
**Tested up to:** WordPress 6.9 

---

## Table of Contents

1. [Overview](#overview)
2. [Installation & Setup](#installation--setup)
3. [Core Features](#core-features)
4. [Shortcodes Reference](#shortcodes-reference)
5. [Admin Configuration](#admin-configuration)
6. [Player Usage Guide](#player-usage-guide)
7. [Tournament Mode](#tournament-mode)
8. [Player Profiles & Social Features](#player-profiles--social-features)
9. [AI Features](#ai-features)
10. [Deck Management](#deck-management)
11. [Analytics & Statistics](#analytics--statistics)
12. [Developer Reference](#developer-reference)
13. [Database Schema](#database-schema)
14. [Troubleshooting](#troubleshooting)

---

## Overview

MTG Life Counter is a comprehensive WordPress plugin that provides a complete Magic: The Gathering gameplay and tournament management ecosystem. It includes:

- **Life Counter** - Browser-based life tracking for 2-8 players with 50+ format support
- **Tournament Mode** - Swiss, Single/Double Elimination, Round Robin, Ladder, and Season tournaments
- **Player Profiles** - Personal dashboards, achievements, friends system, and activity feeds
- **AI Features** - Gameplay insights and mentor chatbot powered by Claude, OpenAI, or Hugging Face
- **Deck Management** - Deck builder, archetype detection, and deck sharing
- **Analytics** - Comprehensive game statistics with charts and visualizations
- **State Management** - Auto-save, game sharing, and spectator mode

### Plugin Architecture

```
mtg-life-counter/
├── mtg-life-counter.php          # Main plugin file (2,479 lines)
├── mtg-life-counter-mp.php       # Multiplayer features (1,050 lines)
├── mtg-life-counter-tournament.php # Tournament bootstrap
├── includes/                      # PHP classes (24 files)
│   ├── class-analytics.php
│   ├── class-database.php
│   ├── class-ai-insights.php
│   ├── class-ai-mentor.php
│   ├── class-deck-archetype.php
│   ├── class-state-manager.php
│   ├── class-preset-manager.php
│   ├── class-export-manager.php
│   ├── tournament/               # Tournament system
│   │   ├── class-tournament-manager.php
│   │   ├── class-tournament-db.php
│   │   ├── class-tournament-ajax.php
│   │   └── class-tournament-shortcodes.php
│   └── player-profiles/          # Player profile system
│       ├── class-player-profiles.php
│       ├── class-achievements.php
│       ├── class-friends.php
│       ├── class-avatar-handler.php
│       └── class-profile-shortcodes-v3.php
├── assets/
│   ├── css/                      # Stylesheets (12 files)
│   └── js/                       # JavaScript (16 files)
└── templates/                    # PHP templates (17 files)
```

**Total Plugin Size:** ~1.5MB  
**Lines of PHP Code:** ~16,945  
**Database Tables:** 14 custom tables

---

## Installation & Setup

### Requirements

- **WordPress:** 5.8 or higher
- **PHP:** 7.4 or higher
- **MySQL:** 5.6 or higher
- **Browser:** Modern browser with JavaScript enabled

### Installation Steps

1. **Upload Plugin**
   ```
   Upload the mtg-life-counter folder to /wp-content/plugins/
   ```

2. **Activate Plugin**
   - Go to WordPress Admin → Plugins
   - Find "MTG Life Counter"
   - Click "Activate"

3. **Database Installation**
   - Plugin automatically creates 14 database tables on activation
   - Tables are prefixed with `{wp_prefix}_mtg_lc_`

4. **Verify Installation**
   - Navigate to **Settings → MTG Life Counter**
   - You should see multiple tabs: General, State Management, Analytics, AI Insights, Player Profiles, Tournament

5. **Add Life Counter to Page**
   ```
   Create a new page
   Add the shortcode: [mtg_life_counter]
   Publish and view
   ```

### Initial Configuration

#### Step 1: Configure Formats

1. Go to **Settings → MTG Life Counter → General**
2. Review default formats (50+ included)
3. Add custom formats if needed
4. Set default starting life totals

#### Step 2: Enable Features

**Game State Persistence:**
```
Settings → MTG Life Counter → State Management
☑ Enable state persistence
Choose: Local storage / Server / Both
Set session duration (default: 30 days)
```

**Analytics Tracking:**
```
Settings → MTG Life Counter → Analytics
☑ Enable analytics tracking
Choose retention period
Configure privacy settings
```

**Player Profiles:**
```
Settings → MTG Life Counter → Player Profiles
☑ Enable player profiles
☑ Enable achievements
Configure privacy defaults
Customize color scheme
```

**AI Features (Optional):**
```
Settings → MTG Life Counter → AI Insights
☑ Enable AI enhancement
Choose provider: Claude / OpenAI / Hugging Face / None
Enter API key
Set monthly limit
```

#### Step 3: Create Pages

**Recommended Page Structure:**
```
Play MTG           [mtg_life_counter]
My Profile         [mtg_player_profile]
Leaderboard        [mtg_leaderboard]
Tournaments        [mtg_tournament_list]
Deck Builder       [mtg_deck_builder]
AI Insights        [mtg_ai_insights]
Login/Register     [mtg_login] [mtg_register]
```

---

## Core Features

### Life Counter

The core life tracking interface supports 2-8 players simultaneously.

#### Supported Formats (50+)

**Official Constructed Formats:**
- Commander (EDH) - 40 life, 4 players
- Standard - 20 life
- Modern - 20 life
- Legacy - 20 life
- Vintage - 20 life
- Pioneer - 20 life
- Historic - 20 life
- Pauper - 20 life
- Brawl - 25 life (30 in multiplayer)

**Multiplayer Formats:**
- Two-Headed Giant - 30 life (shared)
- Emperor - 20 life
- Free-for-All - 20 life
- Archenemy - Varies by role

**Limited Formats:**
- Booster Draft - 20 life
- Sealed Deck - 20 life
- Cube Draft - 20 life
- Rochester Draft - 20 life

**Casual/Variant Formats:**
- Oathbreaker - 20 life
- Tiny Leaders - 25 life
- Duel Commander - 20 life
- Canadian Highlander - 20 life
- Planechase - 20 life
- Vanguard - Varies
- And 25+ more...

#### Counter Types

The life counter tracks multiple counter types:

**Life Totals** - Primary life tracking
**Poison Counters** - Infect damage (10 = lose)
**Energy Counters** - Energy mechanic
**Experience Counters** - Commander experience
**+1/+1 Counters** - Creature buffs
**Loyalty Counters** - Planeswalker abilities
**Commander Damage** - Tracks damage from each commander

#### UI Features

**Increment/Decrement Buttons**
- Single tap: ±1 life
- Long press: ±5 life (hold for 0.5s)
- Custom amounts via input field

**Undo/Redo System**
- Unlimited undo/redo history
- Keyboard shortcuts: Ctrl+Z (undo), Ctrl+Y (redo)
- Visual history log with timestamps

**Action History**
- Complete log of all life changes
- Timestamps for each action
- Filter by player
- Export to CSV

**Dice Roller**
- Supported dice: D4, D6, D8, D10, D12, D20, D100
- Multiple dice rolls
- Roll history
- Animations

**Themes**
- Auto (system preference)
- Dark mode
- Light mode
- High contrast (accessibility)

**Layouts**
- Linear (horizontal row)
- Grid (2x2, 2x3, etc.)
- Circle (players around table)
- Teams (Two-Headed Giant)

### Multiplayer Features

#### Turn Tracking
- Visual indicator of active player
- Automatic rotation
- Manual turn advancement
- Turn counter

#### Player Management
- Add/remove players dynamically
- Player elimination tracking
- Name customization
- Color assignment

#### Tokens
- **Monarch** - Draw extra card each turn
- **Initiative** - Venture into Undercity
- Token ownership tracking
- Visual indicators

#### Team Play (Two-Headed Giant)
- Shared life pools
- Team color coding
- Combined stats
- Team victory conditions

---

## Shortcodes Reference

### Primary Life Counter

#### `[mtg_life_counter]`

The main life counter interface.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `players` | integer | 2 | Number of players (2-8) |
| `format` | string | "commander" | Format slug |
| `starting_life` | integer | (from format) | Override starting life |
| `theme` | string | "auto" | Theme: auto/dark/light/high-contrast |
| `undo` | boolean | true | Enable undo/redo |
| `history` | boolean | true | Show action history |
| `turns` | boolean | false | Enable turn tracking |
| `tokens` | boolean | false | Enable special tokens |
| `layout` | string | "linear" | Layout: linear/grid/circle/teams |
| `persist` | boolean | false | Enable auto-save |
| `persist_mode` | string | "local" | Save location: local/server/both |
| `resume` | string | "auto" | Resume behavior: auto/prompt/never |
| `shareable` | boolean | false | Enable game sharing |

**Examples:**

```
Basic 4-player Commander game:
[mtg_life_counter players="4" format="commander"]

2-player Modern with dark theme and persistence:
[mtg_life_counter players="2" format="modern" theme="dark" persist="true"]

6-player EDH with turn tracking and sharing:
[mtg_life_counter players="6" format="commander" turns="true" shareable="true"]

Custom starting life:
[mtg_life_counter players="2" starting_life="30"]

Full-featured multiplayer:
[mtg_life_counter players="4" format="commander" theme="dark" undo="true" history="true" turns="true" tokens="true" persist="true" shareable="true"]
```

### Tournament Shortcodes

#### `[mtg_tournament]`

Full tournament interface with standings, pairings, enrollment, and match reporting.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `slug` | string | Yes | Tournament slug/ID |

**Example:**
```
[mtg_tournament slug="fnm-january-2026"]
```

**Features:**
- Real-time standings table
- Current round pairings
- Enroll/drop buttons
- Match result submission
- Admin controls (if admin)

---

#### `[mtg_tournament_standings]`

Standalone standings table only.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `slug` | string | Yes | Tournament slug/ID |
| `limit` | integer | No | Number of players to show |

**Example:**
```
[mtg_tournament_standings slug="fnm-january-2026"]
[mtg_tournament_standings slug="fnm-january-2026" limit="10"]
```

---

#### `[mtg_tournament_bracket]`

Current round pairings/bracket only.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `slug` | string | Yes | Tournament slug/ID |

**Example:**
```
[mtg_tournament_bracket slug="fnm-january-2026"]
```

---

#### `[mtg_ladder]`

Ladder standings with inline match recording.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `slug` | string | Yes | Ladder slug/ID |

**Example:**
```
[mtg_ladder slug="weekly-ladder"]
```

**Features:**
- Perpetual standings (no rounds)
- Any enrolled player can report matches anytime
- Win/loss streaks
- Match history per player

---

#### `[mtg_season]`

Season standings with date-based progress bar.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `slug` | string | Yes | Season slug/ID |

**Example:**
```
[mtg_season slug="winter-2026"]
```

**Features:**
- Start/end dates
- Progress bar showing time remaining
- Season standings
- Winner declaration when season ends

---

#### `[mtg_tournament_list]`

Grid of all tournaments with filters.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `status` | string | "all" | Filter: all/upcoming/active/completed |
| `type` | string | "all" | Filter by type: swiss/elimination/ladder/season |
| `limit` | integer | 12 | Number to show |

**Example:**
```
[mtg_tournament_list]
[mtg_tournament_list status="active"]
[mtg_tournament_list type="swiss" limit="6"]
```

---

#### `[mtg_my_results]`

Current user's personal match history in a tournament.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `slug` | string | Yes | Tournament slug/ID |

**Example:**
```
[mtg_my_results slug="fnm-january-2026"]
```

**Features:**
- Shows only matches where current user participated
- Win/loss record
- Opponent names
- Game scores (e.g., 2-1)

---

### Player Profile Shortcodes

#### `[mtg_login]`

Login form.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `redirect` | string | (current page) | Redirect URL after login |
| `register` | boolean | true | Show register link |

**Example:**
```
[mtg_login]
[mtg_login redirect="/my-profile"]
[mtg_login register="false"]
```

---

#### `[mtg_register]`

Registration form.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `redirect` | string | (current page) | Redirect URL after registration |

**Example:**
```
[mtg_register]
[mtg_register redirect="/welcome"]
```

---

#### `[mtg_player_profile]`

Full player profile dashboard.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `user_id` | integer/string | "current" | User ID or "current" |

**Example:**
```
[mtg_player_profile]
[mtg_player_profile user_id="current"]
[mtg_player_profile user_id="42"]
```

**Features:**
- Profile photo/avatar
- Win/loss statistics
- Format performance
- Recent games
- Achievements
- Friends list
- Match history
- Deck collection

---

#### `[mtg_recent_games]`

Widget showing recent games for a player.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `user_id` | integer/string | "current" | User ID or "current" |
| `limit` | integer | 10 | Number of games to show |
| `format` | string | (all) | Filter by format |

**Example:**
```
[mtg_recent_games]
[mtg_recent_games limit="5"]
[mtg_recent_games user_id="42" format="commander"]
```

---

#### `[mtg_player_stats]`

Statistics widget.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `user_id` | integer/string | "current" | User ID or "current" |

**Example:**
```
[mtg_player_stats]
[mtg_player_stats user_id="42"]
```

**Displays:**
- Total games played
- Win rate percentage
- Total playtime
- Most played format
- Current win/loss streak

---

#### `[mtg_achievements]`

Achievement badges for a player.

**Example:**
```
[mtg_achievements]
[mtg_achievements user_id="42"]
```

**Achievement Types:**
- First Win
- 10 Wins, 50 Wins, 100 Wins
- Commander Master (100 Commander games)
- Streak achievements (3-win, 5-win, 10-win streaks)
- Format achievements
- Tournament victories

---

#### `[mtg_match_history]`

Detailed match history table.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | 20 | Number of matches |
| `format` | string | (all) | Filter by format |

**Example:**
```
[mtg_match_history]
[mtg_match_history limit="10" format="modern"]
```

---

#### `[mtg_leaderboard]`

Global leaderboard of all players.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | 50 | Number of players |
| `format` | string | (all) | Filter by format |
| `sort` | string | "wins" | Sort: wins/winrate/games |

**Example:**
```
[mtg_leaderboard]
[mtg_leaderboard limit="10" sort="winrate"]
[mtg_leaderboard format="commander"]
```

---

#### `[mtg_friends_list]`

Friends list for current user.

**Example:**
```
[mtg_friends_list]
```

**Features:**
- Send friend requests
- Accept/decline requests
- Remove friends
- View friend profiles
- Head-to-head records

---

#### `[mtg_opponents]`

List of opponents with head-to-head records.

**Example:**
```
[mtg_opponents]
```

**Shows:**
- All players you've faced
- Win/loss record against each
- Last game date
- Most common format

---

#### `[mtg_activity_feed]`

Social activity feed.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | 20 | Number of activities |

**Example:**
```
[mtg_activity_feed]
[mtg_activity_feed limit="10"]
```

**Activity Types:**
- Game completions
- Achievement unlocks
- Friend requests
- Tournament enrollments
- Deck creations

---

### Deck Management Shortcodes

#### `[mtg_deck_builder]`

Interactive deck builder.

**Example:**
```
[mtg_deck_builder]
```

**Features:**
- Search cards via Scryfall API
- Add/remove cards
- Organize by category (lands, creatures, etc.)
- Save decks
- Archetype detection
- Mana curve visualization

---

#### `[mtg_deck_analyzer]`

Analyze a deck for archetype and stats.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `deck_id` | integer | Yes | Deck ID to analyze |

**Example:**
```
[mtg_deck_analyzer deck_id="123"]
```

**Analysis Includes:**
- Detected archetype
- Mana curve
- Card type distribution
- Color identity
- Average CMC
- Recommendations

---

#### `[mtg_my_decks]`

User's deck collection.

**Example:**
```
[mtg_my_decks]
```

**Features:**
- Grid of all user's decks
- Quick view/edit/delete
- Archetype badges
- Deck statistics

---

#### `[mtg_deck_view]`

Display a single deck.

**Attributes:**

| Attribute | Type | Required | Description |
|-----------|------|----------|-------------|
| `deck_id` | integer | Yes | Deck ID to display |

**Example:**
```
[mtg_deck_view deck_id="123"]
```

---

#### `[mtg_archetype_guide]`

Guide to all supported archetypes.

**Example:**
```
[mtg_archetype_guide]
```

**Archetypes Documented:**
- Aggro (Red Deck Wins, White Weenie)
- Control (Blue Control, Esper Control)
- Combo (Storm, Splinter Twin)
- Midrange (Jund, Abzan)
- Ramp (Green Ramp, Tron)
- Tribal (Elves, Goblins, Merfolk)
- And 30+ more...

---

### AI Feature Shortcodes

#### `[mtg_ai_insights]`

AI-powered gameplay insights dashboard.

**Example:**
```
[mtg_ai_insights]
```

**Insights Provided:**
- **Temporal Patterns** - Best playing times (e.g., "You win 65% of evening games")
- **Game Duration** - Optimal game lengths
- **Format Recommendations** - Suggested formats to try
- **Performance Trends** - Win rate over time
- **Win Streaks** - Celebration of streaks
- **Improvement Tips** - Personalized advice

**Requirements:**
- User must be logged in
- Requires at least 10 games played
- AI enhancement optional (works with SQL-only mode)

---

#### `[mtg_ai_mentor]`

AI chatbot mentor ("The Oracle").

**Example:**
```
[mtg_ai_mentor]
```

**Features:**
- Natural language conversation
- Ask questions about your performance
- Get strategic advice
- Format recommendations
- Rule clarifications
- Meta analysis

**Example Questions:**
- "What's my best format?"
- "Why am I losing more lately?"
- "Should I try Modern?"
- "How can I improve my Commander win rate?"

**Requirements:**
- User must be logged in
- AI provider must be configured (Claude/OpenAI/Hugging Face)

---

#### `[mtg_insights_badge]`

Small badge showing key insight.

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `type` | string | "winrate" | Insight type: winrate/streak/format |

**Example:**
```
[mtg_insights_badge]
[mtg_insights_badge type="streak"]
[mtg_insights_badge type="format"]
```

---

### Multiplayer Table Shortcode

#### `[mtg_table]`

Alternative multiplayer interface with enhanced features.

**Attributes:** (Same as `[mtg_life_counter]`)

**Example:**
```
[mtg_table players="4" format="commander"]
```

**Additional Features:**
- Enhanced visual layout
- Better mobile support
- Optimized for tablets

---

## Admin Configuration

### Settings Tabs

Navigate to **Settings → MTG Life Counter** to access configuration.

#### General Tab

**Custom Formats**
- Add/edit/delete formats
- Set default starting life
- Define format rules
- Assign color schemes

**Default Settings**
- Default number of players
- Default format
- Default theme
- Enable/disable features globally

**Format Examples:**
```
Format: Commander
Starting Life: 40
Default Players: 4
Allows Tokens: Yes
```

---

#### State Management Tab

**Persistence Settings**

| Setting | Options | Description |
|---------|---------|-------------|
| Enable Persistence | On/Off | Master switch |
| Storage Mode | Local / Server / Both | Where to save |
| Session Duration | Days | How long to keep saved games |
| Auto-resume | On/Off | Resume games automatically |

**Cleanup**
- Manually clear expired sessions
- Set retention period
- View storage usage

---

#### Analytics Tab

**Tracking Settings**

| Setting | Options | Description |
|---------|---------|-------------|
| Enable Analytics | On/Off | Master switch |
| Track Anonymous Users | On/Off | Track non-logged-in users |
| Retention Period | Days | How long to keep analytics data |
| Privacy Mode | On/Off | Anonymize IP addresses |

**Data Management**
- View analytics dashboard
- Export to CSV
- Clear all analytics data
- View database size

**Charts Available:**
- Games over time (line chart)
- Games by format (bar chart)
- Games by player count (pie chart)
- Activity heatmap (day/hour)
- Win/loss ratio
- Average game duration
- Top players

---

#### AI Insights Tab

**AI Provider Configuration**

| Provider | API Key Required | Cost | Quality |
|----------|------------------|------|---------|
| None (SQL only) | No | Free | Basic insights |
| Hugging Face | Yes | Free (30K/month) | Good |
| OpenAI GPT-3.5 | Yes | ~$2-3/month | Excellent |
| Anthropic Claude | Yes | ~$1-2/month | Best |

**Settings:**

```
☑ Enable AI Enhancement
Provider: [Claude Sonnet ▼]
API Key: [sk-ant-...]
Monthly Limit: 1000 requests
Temperature: 0.7 (creativity level)
Max Tokens: 500
```

**SQL-Only Mode (No AI):**
- Still provides insights
- Uses database queries only
- No natural language polish
- Completely free
- Example: "You have a 62% win rate in Commander"

**AI-Enhanced Mode:**
- Natural language insights
- More detailed analysis
- Conversational mentor
- Example: "Your Commander skills are impressive! You've maintained a stellar 62% win rate across 45 games, particularly strong in evening sessions."

---

#### Player Profiles Tab

**Profile Settings**

| Setting | Options | Description |
|---------|---------|-------------|
| Enable Profiles | On/Off | Master switch |
| Enable Achievements | On/Off | Unlock achievements |
| Require Login | On/Off | Force login to play |
| Default Privacy | Public/Friends/Private | Default profile visibility |

**Avatar Settings**
- Maximum upload size (MB)
- Allowed formats (JPG, PNG, GIF)
- Generate default avatars

**Achievement Configuration**
- Enable/disable specific achievements
- Customize unlock criteria
- Set point values

**Color Scheme Customization**

Customize 10 key colors used throughout profiles:

| Color | Default | Usage |
|-------|---------|-------|
| Primary | `#0066cc` | Main brand color |
| Secondary | `#6c757d` | Secondary elements |
| Success | `#28a745` | Wins, positive actions |
| Danger | `#dc3545` | Losses, warnings |
| Warning | `#ffc107` | Alerts |
| Info | `#17a2b8` | Information |
| Win Bg | `#d4edda` | Win backgrounds |
| Loss Bg | `#f8d7da` | Loss backgrounds |
| Border | `#dee2e6` | Borders |
| Background | `#f8f9fa` | Card backgrounds |

---

#### Tournament Tab

**Create Tournament**

```
Tournament Name: Friday Night Magic - January 2026
Slug: fnm-january-2026
Type: Swiss ▼
Format: Modern ▼
Best of: 3 ▼
Rounds: 4 (auto-calculated for Swiss)
Start Date: 2026-01-17
Entry Fee: $5
Prize Pool: $100
Max Players: 32
Status: Upcoming ▼
```

**Tournament Types:**

| Type | Description | Features |
|------|-------------|----------|
| Swiss | WotC-standard pairings | OMW% tiebreakers, bye support, points bracket |
| Single Elimination | Bracket knockout | Seeding, advancement |
| Double Elimination | Losers bracket | Winners + losers brackets |
| Round Robin | Everyone plays everyone | Berger algorithm |
| Ladder | Perpetual standings | Open enrollment, report anytime |
| Season | Time-bounded ladder | Start/end dates, progress bar |

**Tournament Management**

Once created, admin can:
- Start/pause/end tournament
- Advance to next round
- Manually adjust pairings
- Add/remove players
- Edit match results
- Assign byes
- Delete tournament
- View full standings
- Export results to CSV

**Swiss Pairing Algorithm:**
- Points bracket (players with same points paired together)
- Prior opponent avoidance (never face same opponent twice)
- Bye assignment (odd player count, assigned to lowest-ranked eligible player)
- OMW% tiebreaker (Opponent Match Win Percentage)

---

## Player Usage Guide

### Getting Started

#### Creating an Account

1. Navigate to login page with `[mtg_login]` or `[mtg_register]`
2. Fill in registration form:
   - Username
   - Email
   - Password
3. Click "Register"
4. Verify email (if configured)
5. You're ready to play!

---

### Playing a Game

#### Starting a Game

1. Go to page with `[mtg_life_counter]` shortcode
2. Select format from dropdown
3. Adjust player count if needed
4. Click "Start Game" or "New Game"
5. Life counters initialize to format's starting life

#### During Gameplay

**Changing Life Totals:**
- Click `+` to add 1 life
- Click `-` to subtract 1 life
- **Long press** (hold 0.5s) for ±5 life
- Click life number to enter custom amount

**Tracking Counters:**
- Poison: Click ☠️ icon
- Energy: Click ⚡ icon
- Experience: Click ⭐ icon
- Commander Damage: Click ⚔️ icon

**Using Undo/Redo:**
- Click undo button or press `Ctrl+Z`
- Click redo button or press `Ctrl+Y`
- View action history in sidebar

**Rolling Dice:**
- Click dice icon
- Select die type (D6, D20, etc.)
- View result
- Roll history shown in log

**Ending Game:**
- Click "End Game" button
- Select winner(s)
- Game is recorded to analytics (if logged in)
- Players receive achievements

---

### Using Persistence

**Auto-Save:**

If persist="true":
- Game auto-saves every 30 seconds
- Save to browser localStorage and/or server
- Closing browser preserves game state
- Return later to resume

**Resuming:**

When returning:
- If `resume="auto"`: Game automatically loads
- If `resume="prompt"`: Modal asks "Resume or start new?"
- If `resume="never"`: Always start fresh

**Managing Saved Games:**
- View saved games in profile
- Delete old games
- Export game state to JSON

---

### Sharing Games

**Creating Share Link:**

1. Start a game with `shareable="true"`
2. Click "Share" button in toolbar
3. Click "Generate Link"
4. Share link is created with unique token
5. Copy link or scan QR code

**Spectator Mode:**

Friends who visit share link see:
- Read-only game state
- Real-time updates (polling every 5s)
- All life totals and counters
- Action history
- Cannot modify game

**Revoking Share:**
- Click "Revoke Link" to disable access
- Previous link becomes invalid

---

### Managing Profile

**Editing Profile:**

1. Go to `[mtg_player_profile]` page
2. Click "Edit Profile"
3. Update:
   - Display name
   - Bio
   - Favorite format
   - Location
   - Privacy settings
4. Click "Save"

**Uploading Avatar:**

1. Go to profile page
2. Click on avatar circle
3. Upload image (JPG, PNG, GIF)
4. Crop if needed
5. Save

**Viewing Statistics:**

Your profile shows:
- Total games played
- Win/loss record
- Win rate percentage
- Total playtime
- Most played format
- Format performance breakdown
- Recent games
- Achievements

---

### Earning Achievements

**Automatic Unlocks:**

Achievements unlock automatically when conditions met:

| Achievement | Requirement |
|-------------|-------------|
| First Blood | Win your first game |
| 10 Wins | Win 10 games |
| 50 Wins | Win 50 games |
| 100 Wins | Win 100 games |
| Commander Master | Play 100 Commander games |
| Modern Maestro | Play 100 Modern games |
| Hat Trick | Win 3 games in a row |
| Hot Streak | Win 5 games in a row |
| Unstoppable | Win 10 games in a row |
| Tournament Victor | Win a tournament |
| Social Butterfly | Add 10 friends |

**Viewing Achievements:**

- Visit profile page
- Achievements shown as badges
- Click to see unlock date
- Share to activity feed

---

### Friends & Social

**Adding Friends:**

1. Visit another player's profile
2. Click "Add Friend"
3. They receive friend request
4. When accepted, you're friends

**Managing Friends:**

- View friends list: `[mtg_friends_list]`
- See head-to-head records
- Challenge to games
- Remove friends

**Activity Feed:**

- View friends' activities: `[mtg_activity_feed]`
- See recent games
- Achievement unlocks
- Tournament results

---

### Tournament Participation

#### Enrolling in Tournament

1. Navigate to tournament page with `[mtg_tournament slug="..."]`
2. Click "Enroll" button
3. Enter deck name (optional)
4. Confirm enrollment

#### Playing Tournament Matches

**Swiss/Round Robin:**
1. View pairings for current round
2. Find your opponent
3. Play your match
4. Click "Report Result"
5. Enter game scores (e.g., 2-1)
6. Confirm submission

**Elimination:**
1. Check bracket for your match
2. Play match
3. Report result
4. Winners advance, losers eliminated (or to losers bracket in double elim)

**Ladder/Season:**
1. Find opponent from standings or elsewhere
2. Play match anytime
3. Report result via "Record Match" button
4. Enter opponent, result, and scores

#### Checking Standings

- Standings update in real-time
- Shows: Rank, Player, Match Points, W-L-D, OMW%, Win Streak
- Filter by tournament status

#### Dropping from Tournament

- Click "Drop" button
- Confirm drop
- You're removed from future pairings
- Past results remain

---

## Tournament Mode

### Tournament Types Explained

#### Swiss Pairings

**How it Works:**
- Players paired based on current standings
- Same point totals paired together
- Never play same opponent twice
- Number of rounds: `ceil(log2(players))`
- Top players after all rounds declared winners

**Tiebreakers:**
1. Match Points (3 for win, 1 for draw, 0 for loss)
2. OMW% (Opponent Match Win Percentage)
3. Game Win %
4. Random

**Byes:**
- If odd player count, lowest-ranked eligible player gets bye
- Bye = automatic 2-0-0 win
- Can't receive bye twice

**Best For:**
- FNM (Friday Night Magic)
- Larger tournaments (16+ players)
- Ensuring everyone plays equal rounds

---

#### Single Elimination

**How it Works:**
- Bracket format
- Lose once, you're out
- Winners advance
- Continues until one player remains

**Seeding:**
- Random or manual seeding
- Top seed plays bottom seed

**Best For:**
- Quick tournaments
- Smaller player counts (8-16)
- Playoff-style competition

---

#### Double Elimination

**How it Works:**
- Winners bracket + Losers bracket
- Lose once, drop to losers bracket
- Lose twice, you're out
- Finals: Winners bracket winner vs. Losers bracket winner

**Best For:**
- More forgiving format
- Ensures players get at least 2 matches
- Playoff tournaments

---

#### Round Robin

**How it Works:**
- Every player plays every other player once
- Berger algorithm for optimal scheduling
- Most wins at end declared winner

**Rounds:** `players - 1` (for even count) or `players` (for odd)

**Best For:**
- Small player counts (4-8)
- League play
- Comprehensive competition

---

#### Ladder

**How it Works:**
- Perpetual standings
- No rounds
- Any enrolled player can report match anytime
- Standings ranked by total wins
- Open-ended (no end date)

**Best For:**
- Casual ongoing competition
- Store ladders
- Anytime play

---

#### Season

**How it Works:**
- Like Ladder but time-bounded
- Start date and end date
- Progress bar shows time remaining
- Winner declared when season ends
- Automatically closes at end date

**Best For:**
- Monthly/quarterly competitions
- Themed seasons
- Scheduled competitive periods

---

### Creating Tournaments (Admin)

**Access:** Settings → MTG Life Counter → Tournament

**Step-by-Step:**

1. **Click "Create Tournament"**

2. **Fill Tournament Details:**
   ```
   Name: Friday Night Magic - Modern
   Slug: fnm-modern-jan-17 (auto-generated, editable)
   Type: Swiss
   Format: Modern
   Best of: 3 (Best of 1 or 3)
   Description: Weekly Modern tournament
   ```

3. **Set Schedule:**
   ```
   Start Date: 2026-01-17
   Start Time: 18:00
   (For Season: Also set End Date)
   ```

4. **Configure Settings:**
   ```
   Max Players: 32
   Entry Fee: $5
   Prize Pool: $100
   ```

5. **Set Status:**
   ```
   Status: Upcoming
   (Change to Active when ready to start)
   ```

6. **Click "Create"**

Tournament is now visible on `[mtg_tournament_list]` page.

---

### Managing Tournaments (Admin)

**Starting Tournament:**
1. Wait for players to enroll
2. Set status to "Active"
3. Click "Start Round 1"
4. Pairings are generated

**Advancing Rounds:**
1. Wait for all matches to be reported
2. Click "Advance to Round 2"
3. New pairings generated based on standings

**Editing Tournament:**
- Update details anytime
- Modify max players
- Change prize pool
- Update description

**Ending Tournament:**
1. Final round completed
2. Set status to "Completed"
3. Winners displayed on standings
4. Tournament archived

**Deleting Tournament:**
- Click "Delete" button
- Confirm deletion
- All data removed (pairings, matches, enrollments)

---

### Match Reporting

**For Swiss/Round Robin/Elimination:**

1. Find your pairing
2. Click "Report Result"
3. Fill in form:
   ```
   Winner: Player 1 ▼
   Game Score: 2-1
   (2 games won by winner, 1 by loser)
   
   Or select "Draw" if tied
   ```
4. Click "Submit"

**For Ladder/Season:**

1. Click "Record Match" button
2. Fill in form:
   ```
   Opponent: [Search players] ▼
   Result: I Won / I Lost / Draw
   Game Score: 2-0
   ```
3. Click "Submit"

**Admin Override:**
- Admins can edit/delete any match result
- Useful for correcting mistakes

---

## Player Profiles & Social Features

### Profile Components

#### Public Profile

**URL:** `/player-profile/?user_id=123` or via `[mtg_player_profile user_id="123"]`

**Displays:**
- Avatar
- Display name
- Bio
- Location
- Favorite format
- Statistics card
  - Total games
  - Win rate
  - Playtime
  - Most played format
- Recent games list
- Achievements
- Friends count
- Match history

**Privacy Levels:**
- **Public:** Anyone can view
- **Friends Only:** Only friends can view
- **Private:** Only user can view

---

#### Dashboard (Own Profile)

**URL:** Via `[mtg_player_profile]` when logged in

**Additional Features:**
- Edit profile button
- Upload/change avatar
- View all achievements (including locked)
- Deck collection
- Friends management
- Activity feed
- Statistics graphs

---

### Achievements System

**Achievement Categories:**

1. **Wins**
   - First Blood (1 win)
   - 10 Wins
   - 50 Wins
   - 100 Wins
   - 500 Wins
   - 1000 Wins

2. **Formats**
   - Commander Master (100 Commander games)
   - Modern Maestro (100 Modern games)
   - Standard Star (100 Standard games)
   - Legacy Legend (100 Legacy games)
   - (+ achievements for all formats)

3. **Streaks**
   - Hat Trick (3-win streak)
   - Hot Streak (5-win streak)
   - Unstoppable (10-win streak)
   - Legendary (20-win streak)

4. **Tournament**
   - Tournament Victor (win a tournament)
   - Swiss Expert (10 Swiss tournaments)
   - Bracket King (10 elimination tournaments)

5. **Social**
   - Social Butterfly (10 friends)
   - Popular (50 friends)
   - Rival (play same opponent 10 times)

6. **Special**
   - Collector (create 10 decks)
   - Deckbuilder (create 50 decks)
   - Early Adopter (register in first month)

**Unlocking:**
- Automatic when condition met
- Notification shown
- Badge added to profile
- Activity posted to feed
- Points awarded (if points system enabled)

---

### Friends System

**Friend Request Flow:**

1. Player A visits Player B's profile
2. Player A clicks "Add Friend"
3. Player B receives notification
4. Player B accepts or declines
5. If accepted, mutual friendship created

**Friend Features:**

- View friends list
- Head-to-head records
- Challenge to games (if enabled)
- See friends' activity feed
- Filter leaderboard to friends only
- Private messaging (if enabled)

**Managing Friends:**

- Remove friend (breaks both connections)
- Block user (prevents friend requests)
- Favorites (star important friends)

---

### Activity Feed

**Activity Types:**

| Type | Example |
|------|---------|
| Game Completed | "Alice defeated Bob in Commander (2-1)" |
| Achievement Unlocked | "Charlie unlocked 'Hot Streak' achievement" |
| Friend Added | "Diana and Eve are now friends" |
| Tournament Enrolled | "Frank enrolled in FNM Modern" |
| Tournament Victory | "Grace won Friday Night Magic!" |
| Deck Created | "Henry created a new Goblins deck" |
| Profile Updated | "Iris updated their profile" |

**Feed Filters:**
- All activity
- Friends only
- Own activity
- By type

---

## AI Features

### AI Insights

**How It Works:**

1. **Data Collection** - Plugin tracks all your games (format, duration, result, date/time)
2. **Pattern Detection** - SQL queries find patterns in your data
3. **Insight Generation** - AI (optional) generates natural language insights
4. **Display** - Insights shown on dashboard

**Insight Types:**

#### 1. Temporal Patterns

**Analyzes:** What time of day/week you play best

**SQL Query:**
```sql
SELECT HOUR(game_date) as hour, 
       COUNT(*) as games,
       SUM(CASE WHEN won THEN 1 ELSE 0 END) / COUNT(*) as win_rate
FROM mtg_lc_games
GROUP BY hour
ORDER BY win_rate DESC
```

**Example Output (SQL-only):**
```
You have played 45 games total.
Your best time is 7 PM - 10 PM with 68% win rate (22 games).
Your worst time is 9 AM - 12 PM with 40% win rate (8 games).
```

**Example Output (AI-enhanced):**
```
🌙 Night Owl Alert!

You're crushing it in evening sessions! Your best performance comes 
between 7-10 PM, where you've won 68% of your games. Morning magic 
isn't your strength—you're at 40% before noon. 

Recommendation: Schedule your competitive games for evenings, and 
use mornings for casual play or deck testing.
```

---

#### 2. Game Duration Analysis

**Analyzes:** How game length affects your win rate

**Example Output (AI):**
```
⏱️ Sweet Spot: 25-40 Minutes

Your optimal game duration is 25-40 minutes, where you win 72% of games.
Quick games (<20 min): 55% win rate
Long grinds (>60 min): 48% win rate

Your aggro deck might be outpacing opponents in mid-length games, 
but you lose steam in marathon matches.
```

---

#### 3. Format Recommendations

**Analyzes:** Which formats you excel at and suggests new ones

**Example Output (AI):**
```
🎯 Format Mastery Report

Your Top Formats:
1. Commander: 65% win rate (45 games) ⭐
2. Modern: 58% win rate (32 games)
3. Standard: 52% win rate (18 games)

You Might Enjoy:
- Legacy: Similar to Modern but with higher power level
- Brawl: Commander-style but faster games
- Historic: Digital format with similar meta to Modern

Your Struggle Bus:
- Pauper: 38% win rate (only 8 games—not enough data yet!)
```

---

#### 4. Performance Trends

**Analyzes:** Win rate over time

**Example Output (AI):**
```
📈 You're On Fire!

Your win rate has increased from 52% (3 months ago) to 67% (this month).
That's a 15% improvement! 

What changed?
- You've played 80% more Commander games
- Average game duration decreased by 10 minutes
- You're playing more in evenings

Keep doing what you're doing! This trend suggests you're getting 
better at your preferred format.
```

---

#### 5. Win Streak Detection

**Analyzes:** Celebrates current and best streaks

**Example Output (AI):**
```
🔥 LEGENDARY STREAK!

Current Streak: 8 wins in a row!
Best Streak: 12 wins (November 2025)

You're 4 wins away from tying your personal best. 
Channel that momentum!
```

---

### AI Mentor ("The Oracle")

**Interactive chatbot** that answers questions about your gameplay.

**How to Use:**

1. Add `[mtg_ai_mentor]` to a page
2. Chat interface appears
3. Type questions
4. Oracle responds with personalized insights

**Example Conversations:**

**User:** "What's my best format?"

**Oracle:** "Based on your stats, Commander is definitely your sweet spot! You've got a stellar 65% win rate across 45 games. Modern is your second-best at 58%, but you've played it less. I'd recommend sticking with Commander for competitive play, and using Modern to practice different strategies."

---

**User:** "Why am I losing more lately?"

**Oracle:** "Let me check... Ah, I see the issue. Your win rate dropped from 67% to 51% in the past two weeks. Looking at the data:

1. You've been playing during morning hours (9 AM - 12 PM) more often. Your morning win rate is only 40%, while your evening win rate is 68%.

2. You switched from Commander to Standard, and your Standard win rate is 52% vs. 65% in Commander.

3. Average game duration increased to 55 minutes, which is outside your 'sweet spot' of 25-40 minutes.

Recommendation: Go back to evening Commander games in the 25-40 minute range. That's where you dominate!"

---

**User:** "Should I try Modern?"

**Oracle:** "Great question! Based on your profile:

✅ **Yes, try Modern if:**
- You enjoy fast-paced games (Modern averages 30 min, your sweet spot)
- You like strategic complexity (you're a Commander player, so you can handle it)
- You want to improve your win rate (you're at 58%, plenty of room to grow)

⚠️ **BUT:**
- Modern is expensive to build competitive decks
- The meta changes frequently (requires constant updates)
- Your Commander skills translate well, but the playstyle is different

My Recommendation: Try Modern casually for a month. If your win rate stays above 55% and you enjoy it, invest in a tier-1 deck. Otherwise, stick with Commander where you're already crushing it."

---

**User:** "How can I improve?"

**Oracle:** "Here are 3 actionable tips based on your data:

1. **Play More Evenings**: Your 68% evening win rate crushes your 40% morning rate. Schedule serious games for 7-10 PM.

2. **Target 25-40 Minute Games**: You win 72% of games in this duration. If a game drags past 60 minutes, you're down to 48%. Consider bringing faster decks or playing more aggressively.

3. **Stick to Commander**: You've got a 65% win rate in Commander vs. 52% in Standard and 38% in Pauper. Master one format before expanding.

Bonus: You're on an 8-win streak right now. Don't break the momentum! Play when you're in the zone (evenings, Commander, 25-40 min games)."

---

### Configuring AI

**Provider Options:**

| Provider | API Key | Free Tier | Cost (Est.) | Quality |
|----------|---------|-----------|-------------|---------|
| None | No | N/A | $0/month | Basic (SQL only) |
| Hugging Face | Yes | 30,000 requests/month | $0/month | Good |
| OpenAI GPT-3.5 | Yes | $5 credit signup | $2-3/month | Excellent |
| Anthropic Claude | Yes | $5 credit signup | $1-2/month | Best |

**Getting API Keys:**

**Hugging Face (FREE):**
1. Sign up at https://huggingface.co/
2. Go to Settings → Access Tokens
3. Click "New Token"
4. Copy token
5. Paste in plugin settings

**OpenAI:**
1. Sign up at https://platform.openai.com/
2. Get $5 free credit
3. Create API key
4. Paste in plugin settings

**Anthropic Claude:**
1. Sign up at https://console.anthropic.com/
2. Get $5 free credit
3. Create API key
4. Paste in plugin settings

**Settings:**

```
Provider: Anthropic Claude ▼
API Key: sk-ant-api03-...
Model: claude-3-sonnet ▼
Temperature: 0.7 (0=factual, 1=creative)
Max Tokens: 500 (response length)
Monthly Limit: 1000 requests
```

**Testing:**
- Click "Test Connection" button
- Plugin sends test request
- Verify response

---

## Deck Management

### Deck Builder

**Accessing:** `[mtg_deck_builder]`

**Features:**

1. **Card Search** (via Scryfall API)
   - Search by name
   - Filter by color, type, format legality
   - Auto-complete suggestions
   - View card images

2. **Deck Construction**
   - Add/remove cards
   - Set quantities
   - Organize by category:
     - Lands
     - Creatures
     - Spells
     - Artifacts/Enchantments
   - Sideboard (15 cards)

3. **Deck Stats**
   - Total cards: 60 (or 100 for Commander)
   - Mana curve graph
   - Color distribution pie chart
   - Average CMC
   - Card type breakdown

4. **Archetype Detection**
   - Auto-detects archetype based on cards
   - Assigns archetype badge
   - Provides archetype description

5. **Save/Load**
   - Save deck to database
   - Load saved decks
   - Export to .txt or .json
   - Import from .txt or .json

**Example Workflow:**

1. Click "New Deck"
2. Name deck: "Mono-Red Aggro"
3. Select format: "Standard"
4. Search for "Lightning Bolt"
5. Add 4x Lightning Bolt
6. Continue adding cards
7. View mana curve
8. Deck stats show:
   - Total: 60 cards
   - Avg CMC: 2.1
   - Detected Archetype: "Aggro - Red Deck Wins"
9. Click "Save Deck"

---

### Archetype System

**Supported Archetypes:**

The plugin recognizes 40+ archetypes across all formats.

**Detection Algorithm:**

```
1. Analyze deck composition
2. Count creatures, spells, lands
3. Check average CMC
4. Identify key cards (e.g., Lightning Bolt = aggro)
5. Match pattern to archetype database
6. Assign archetype + confidence score
```

**Major Archetypes:**

| Archetype | Key Traits | Example Decks |
|-----------|------------|---------------|
| **Aggro** | Low CMC, many creatures, fast damage | Red Deck Wins, White Weenie |
| **Control** | Counterspells, removal, card draw, high CMC finishers | Blue Control, Esper Control |
| **Midrange** | Balanced threats, removal, value creatures | Jund, Abzan |
| **Combo** | Cards that synergize for instant win | Storm, Splinter Twin |
| **Ramp** | Mana acceleration, large threats | Green Ramp, Tron |
| **Tempo** | Cheap threats + disruption | Delver, Faeries |
| **Tribal** | Creature type synergy | Elves, Goblins, Merfolk |
| **Burn** | Direct damage spells | Mono-Red Burn |
| **Reanimator** | Graveyard recursion | Reanimator |
| **Tokens** | Token generation | Selesnya Tokens |

**Full List:** See `[mtg_archetype_guide]` shortcode

---

### Deck Sharing

**Public Decks:**
- Set deck visibility to "Public"
- Deck appears in public deck list
- Other users can view and copy

**Private Decks:**
- Only you can see
- Not searchable

**Deck View:**
- `[mtg_deck_view deck_id="123"]`
- Shows full deck list
- Displays stats and archetype
- Mana curve visualization
- Copy to own collection button

---

## Analytics & Statistics

### Analytics Dashboard

**Accessing:** Settings → MTG Life Counter → Analytics

**Overview:**

The analytics dashboard provides comprehensive insights into gameplay across your site.

**Key Performance Indicators (KPIs):**

```
┌─────────────────────┐  ┌─────────────────────┐
│   Total Games       │  │   Total Players     │
│      1,247          │  │        156          │
└─────────────────────┘  └─────────────────────┘

┌─────────────────────┐  ┌─────────────────────┐
│  Avg Game Duration  │  │   Formats Played    │
│    38 minutes       │  │         12          │
└─────────────────────┘  └─────────────────────┘
```

---

**Charts:**

#### 1. Games Over Time (Line Chart)
- X-axis: Date
- Y-axis: Number of games
- Shows trend over selected period
- Identify peak activity days

#### 2. Games by Format (Bar Chart)
- X-axis: Format name
- Y-axis: Game count
- See most popular formats

#### 3. Games by Player Count (Pie Chart)
- Segments: 2-player, 3-player, 4-player, etc.
- Shows distribution of game sizes

#### 4. Activity Heatmap
- Rows: Days of week (Monday - Sunday)
- Columns: Hours of day (0-23)
- Color intensity: Number of games
- Identify peak gaming times

#### 5. Win/Loss Ratio (Pie Chart)
- Wins vs. Losses
- Overall win rate percentage

#### 6. Average Game Duration by Format (Bar Chart)
- Compare game lengths across formats

#### 7. Top Players (Table)
- Ranked list of players by wins
- Shows games played, win rate

---

**Filters:**

| Filter | Options |
|--------|---------|
| Time Period | 7 days / 30 days / 90 days / All time |
| Format | All / Commander / Modern / etc. |
| Player Count | All / 2 players / 4 players / etc. |

---

**Export:**

- **CSV Export** - Download all analytics data
- **JSON Export** - Raw data format
- **PDF Report** - (Planned feature)

**Columns in CSV:**
```
game_id, date, format, players, duration_minutes, winner_id, game_state
```

---

### Event Tracking

**Tracked Events:**

| Event | Description | Data Captured |
|-------|-------------|---------------|
| `game_start` | New game started | Format, player count, timestamp |
| `game_end` | Game completed | Winner, duration, final state |
| `life_change` | Life total changed | Player, amount, new total |
| `counter_change` | Counter modified | Type, amount |
| `undo` | Undo action used | Action reverted |
| `dice_roll` | Dice rolled | Die type, result |
| `format_change` | Format switched mid-game | Old format, new format |

**Privacy:**

- IP addresses can be anonymized (last octet zeroed)
- User IDs optional (track anonymous users or not)
- GDPR-compliant data retention
- Users can request data deletion

---

## Developer Reference

### PHP Classes

#### Core Classes

**MTG_LC_Database** (`includes/class-database.php`)
- Handles all database operations
- Creates/updates tables
- Provides query methods
- Methods:
  - `install()` - Create tables
  - `save_game($data)` - Save game state
  - `get_game($id)` - Retrieve game
  - `delete_expired_games()` - Cleanup

**MTG_LC_State_Manager** (`includes/class-state-manager.php`)
- Manages game state persistence
- Local storage and server storage
- Methods:
  - `save_state($state)` - Save to DB or localStorage
  - `load_state($game_id)` - Load game state
  - `generate_share_token()` - Create share link
  - `revoke_share_token($token)` - Disable link

**MTG_LC_Preset_Manager** (`includes/class-preset-manager.php`)
- Manages saved presets (favorite setups)
- Methods:
  - `save_preset($name, $config)` - Save preset
  - `load_preset($id)` - Load preset
  - `get_user_presets()` - List user's presets

**MTG_LC_Export_Manager** (`includes/class-export-manager.php`)
- Handles CSV/JSON exports
- Methods:
  - `export_to_csv($game_id)` - Export game
  - `export_to_json($game_id)` - Export as JSON
  - `import_from_json($json_data)` - Import game

---

#### Analytics Classes

**MTG_LC_Analytics** (`includes/class-analytics.php`)
- Core analytics engine
- Methods:
  - `track_event($event_type, $data)` - Record event
  - `get_games_over_time($period)` - Time series data
  - `get_format_stats()` - Format breakdown
  - `get_top_players($limit)` - Leaderboard
  - `generate_heatmap()` - Activity heatmap
  - `is_enabled()` - Check if tracking enabled
  - `bust_cache()` - Clear cached stats

---

#### AI Classes

**MTG_LC_AI_Insights** (`includes/class-ai-insights.php`)
- Generates gameplay insights
- Methods:
  - `get_temporal_patterns($user_id)` - Time analysis
  - `get_duration_analysis($user_id)` - Game length patterns
  - `get_format_recommendations($user_id)` - Format suggestions
  - `get_performance_trends($user_id)` - Win rate trends
  - `get_win_streaks($user_id)` - Streak detection
  - `enhance_with_ai($insight, $user_data)` - AI polish (optional)

**MTG_LC_AI_Mentor** (`includes/class-ai-mentor.php`)
- Chatbot mentor
- Methods:
  - `chat($user_id, $message)` - Process chat message
  - `get_context($user_id)` - Get user's game data
  - `generate_response($message, $context)` - AI response

---

#### Player Profile Classes

**MTG_LC_Player_Profiles** (`includes/player-profiles/class-player-profiles.php`)
- Core profile system
- Methods:
  - `get_profile($user_id)` - Get profile data
  - `update_profile($user_id, $data)` - Update profile
  - `get_stats($user_id)` - Get statistics
  - `record_game($user_id, $game_data)` - Record game participation

**MTG_LC_Achievements** (`includes/player-profiles/class-achievements.php`)
- Achievement system
- Methods:
  - `check_achievements($user_id)` - Check for unlocks
  - `unlock_achievement($user_id, $achievement_id)` - Grant achievement
  - `get_user_achievements($user_id)` - List achievements

**MTG_LC_Friends** (`includes/player-profiles/class-friends.php`)
- Friends system
- Methods:
  - `send_request($from_user_id, $to_user_id)` - Send friend request
  - `accept_request($request_id)` - Accept request
  - `get_friends($user_id)` - List friends
  - `get_head_to_head($user1_id, $user2_id)` - Get record

**MTG_LC_Avatar_Handler** (`includes/player-profiles/class-avatar-handler.php`)
- Avatar uploads
- Methods:
  - `upload_avatar($user_id, $file)` - Upload image
  - `get_avatar_url($user_id)` - Get avatar URL
  - `delete_avatar($user_id)` - Remove avatar

---

#### Tournament Classes

**MTG_LC_Tournament_Manager** (`includes/tournament/class-tournament-manager.php`)
- Core tournament logic
- Methods:
  - `create_tournament($data)` - Create tournament
  - `start_tournament($tournament_id)` - Begin tournament
  - `advance_round($tournament_id)` - Next round
  - `generate_pairings($tournament_id, $round)` - Pairing algorithm
  - `calculate_standings($tournament_id)` - Rank players

**MTG_LC_Tournament_DB** (`includes/tournament/class-tournament-db.php`)
- Tournament database operations
- Methods:
  - `get_tournament($id)` - Retrieve tournament
  - `save_match($match_data)` - Record match result
  - `get_standings($tournament_id)` - Get current standings
  - `enroll_player($tournament_id, $user_id, $deck)` - Enroll

**MTG_LC_Tournament_Ajax** (`includes/tournament/class-tournament-ajax.php`)
- AJAX handlers for tournament actions
- Endpoints:
  - `mtg_tournament_enroll` - Enroll player
  - `mtg_tournament_drop` - Drop player
  - `mtg_tournament_report_match` - Submit result
  - `mtg_tournament_advance_round` - Admin advance

---

#### Deck Classes

**MTG_LC_Deck_Archetype** (`includes/class-deck-archetype.php`)
- Deck archetype detection
- Methods:
  - `detect_archetype($deck_data)` - Analyze deck
  - `get_archetype_info($archetype_name)` - Get description
  - `get_all_archetypes()` - List all archetypes

**MTG_LC_Deck_Shortcodes** (`includes/class-deck-shortcodes.php`)
- Deck-related shortcodes
- Shortcodes:
  - `[mtg_deck_builder]`
  - `[mtg_deck_analyzer]`
  - `[mtg_my_decks]`
  - `[mtg_deck_view]`
  - `[mtg_archetype_guide]`

**MTG_LC_Deck_Ajax** (`includes/class-deck-ajax.php`)
- AJAX handlers for deck operations
- Endpoints:
  - `mtg_deck_save` - Save deck
  - `mtg_deck_delete` - Delete deck
  - `mtg_deck_search_cards` - Search Scryfall

---

### JavaScript Files

#### Core Life Counter Scripts

**mtg-life-counter.js** (311 lines)
- Basic life counter functionality
- Player management
- Life total changes
- Undo/redo
- Dice roller

**mtg-life-counter-ui.js** (1,456 lines)
- Advanced UI features
- Turn tracking
- Token management
- Theme switching
- Layout management
- Action history

**mtg-life-counter-mp.js** (782 lines)
- Multiplayer features
- Team play (Two-Headed Giant)
- Spectator mode
- Player elimination

---

#### State & Sharing Scripts

**assets/js/state-persistence.js**
- Auto-save functionality
- LocalStorage management
- Server sync
- Resume game logic

**assets/js/share-ui.js**
- Share button
- Generate share links
- QR code generation
- Revoke links

**assets/js/preset-ui.js**
- Preset management UI
- Save/load presets
- Preset dropdown

---

#### Analytics Scripts

**assets/js/analytics-tracker.js**
- Front-end event tracking
- Sends events to server
- Batches requests
- Privacy-aware

**assets/js/analytics-admin.js**
- Admin analytics dashboard
- Chart rendering (Chart.js)
- Filter controls
- CSV export

---

#### AI Scripts

**assets/js/ai-insights.js**
- Insights dashboard
- Fetch insights from server
- Display charts
- Refresh data

**assets/js/ai-mentor.js**
- Chatbot UI
- Message handling
- Conversation history
- Typing indicators

---

#### Player Profile Scripts

**assets/js/player-profiles/player-profiles.js**
- Core profile functionality
- Profile editing
- Stats display

**assets/js/player-profiles/phase2-profiles.js**
- Achievement display
- Match history
- Leaderboards

**assets/js/player-profiles/phase3-social.js**
- Friends list
- Activity feed
- Friend requests
- Head-to-head records

---

#### Tournament Scripts

**assets/js/tournament.js**
- Tournament UI
- Enrollment
- Match reporting
- Standings refresh
- Admin controls

---

#### Deck Scripts

**assets/js/deck-archetype.js**
- Deck builder UI
- Card search (Scryfall API)
- Deck stats
- Mana curve chart
- Archetype detection display

---

### CSS Files

#### Core Styles

**mtg-life-counter.css** (148 lines)
- Basic life counter styles
- Button styles
- Player containers

**mtg-life-counter-ui.css** (1,356 lines)
- Advanced UI styles
- Action history
- Dice roller
- Themes (dark, light, high contrast)
- Responsive design

**mtg-life-counter-mp.css** (540 lines)
- Multiplayer styles
- Team layouts
- Turn indicators
- Token badges

---

#### Feature Styles

**assets/css/analytics.css**
- Analytics dashboard
- Chart containers
- KPI cards

**assets/css/ai-insights.css**
- Insights dashboard
- Insight cards
- Badges

**assets/css/ai-mentor.css**
- Chatbot UI
- Message bubbles
- Chat container

**assets/css/tournament.css**
- Tournament tables
- Pairings
- Standings
- Match reporting forms

**assets/css/deck-archetype.css**
- Deck builder
- Card list
- Mana curve chart
- Archetype badges

**assets/css/state-features.css**
- Share modal
- QR codes
- Preset dropdown

---

#### Player Profile Styles

**assets/css/player-profiles/player-profiles.css**
- Basic profile layout
- Stats cards
- Avatar display

**assets/css/player-profiles/phase2-profiles.css**
- Achievement badges
- Match history table
- Leaderboard

**assets/css/player-profiles/phase3-social.css**
- Friends list
- Activity feed
- Friend request buttons

---

### AJAX Endpoints

All AJAX actions use WordPress `admin-ajax.php` with nonce verification.

**State Management:**
- `mtg_lc_save_state` - Save game state
- `mtg_lc_load_state` - Load game state
- `mtg_lc_generate_share` - Create share link
- `mtg_lc_revoke_share` - Revoke share link

**Analytics:**
- `mtg_lc_analytics_track` - Track event
- `mtg_lc_analytics_get_data` - Fetch analytics
- `mtg_lc_analytics_clear` - Clear all analytics

**AI:**
- `mtg_lc_ai_insights` - Get insights
- `mtg_lc_ai_mentor_chat` - Chat with mentor

**Profiles:**
- `mtg_lc_profile_update` - Update profile
- `mtg_lc_profile_upload_avatar` - Upload avatar
- `mtg_lc_achievement_check` - Check achievements

**Friends:**
- `mtg_lc_friend_request` - Send request
- `mtg_lc_friend_accept` - Accept request
- `mtg_lc_friend_remove` - Remove friend

**Tournament:**
- `mtg_tournament_enroll` - Enroll player
- `mtg_tournament_drop` - Drop player
- `mtg_tournament_report_match` - Report result
- `mtg_tournament_advance_round` - Advance (admin)
- `mtg_tournament_get_standings` - Fetch standings

**Decks:**
- `mtg_deck_save` - Save deck
- `mtg_deck_delete` - Delete deck
- `mtg_deck_search` - Search Scryfall

---

### Hooks & Filters

**Actions:**

```php
// Initialization
do_action('mtg_lc_init');
do_action('mtg_lc_after_init');

// Game Events
do_action('mtg_lc_game_start', $game_id, $data);
do_action('mtg_lc_game_end', $game_id, $winner_id);

// Profile Events
do_action('mtg_lc_profile_updated', $user_id);
do_action('mtg_lc_achievement_unlocked', $user_id, $achievement_id);

// Tournament Events
do_action('mtg_tournament_created', $tournament_id);
do_action('mtg_tournament_started', $tournament_id);
do_action('mtg_tournament_round_advanced', $tournament_id, $round);
do_action('mtg_tournament_completed', $tournament_id);
```

**Filters:**

```php
// Modify formats
$formats = apply_filters('mtg_lc_formats', $formats);

// Modify starting life
$life = apply_filters('mtg_lc_starting_life', $life, $format);

// Modify achievements
$achievements = apply_filters('mtg_lc_achievements', $achievements);

// Modify archetypes
$archetypes = apply_filters('mtg_lc_archetypes', $archetypes);

// Modify tournament pairings
$pairings = apply_filters('mtg_tournament_pairings', $pairings, $tournament_id, $round);
```

---

### Extending the Plugin

#### Adding Custom Formats

```php
add_filter('mtg_lc_formats', 'my_custom_formats');

function my_custom_formats($formats) {
    $formats['my_format'] = array(
        'name' => 'My Custom Format',
        'starting_life' => 25,
        'default_players' => 2,
        'supports_tokens' => true,
    );
    return $formats;
}
```

---

#### Adding Custom Achievements

```php
add_filter('mtg_lc_achievements', 'my_custom_achievements');

function my_custom_achievements($achievements) {
    $achievements['custom_win_100'] = array(
        'name' => 'Century Club',
        'description' => 'Win 100 games in My Custom Format',
        'condition' => function($user_id) {
            // Check if user has 100 wins in custom format
            global $wpdb;
            $count = $wpdb->get_var($wpdb->prepare(
                "SELECT COUNT(*) FROM {$wpdb->prefix}mtg_lc_games 
                 WHERE winner_id = %d AND format = 'my_format'",
                $user_id
            ));
            return $count >= 100;
        },
        'points' => 50,
    );
    return $achievements;
}
```

---

#### Adding Custom Archetypes

```php
add_filter('mtg_lc_archetypes', 'my_custom_archetypes');

function my_custom_archetypes($archetypes) {
    $archetypes['my_archetype'] = array(
        'name' => 'My Archetype',
        'description' => 'Description of my archetype',
        'key_cards' => array('Card Name 1', 'Card Name 2'),
        'detection_rules' => function($deck) {
            // Return true if deck matches archetype
            // $deck = array of card objects
            return false; // placeholder
        },
    );
    return $archetypes;
}
```

---

## Database Schema

The plugin creates 14 custom database tables with prefix `{wp_prefix}_mtg_lc_`.

### Core Tables

#### `mtg_lc_games`

Stores completed games.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `game_data` | LONGTEXT | JSON game state |
| `format` | VARCHAR(50) | Format played |
| `player_count` | INT(11) | Number of players |
| `winner_id` | BIGINT(20) | Winning user ID |
| `duration_minutes` | INT(11) | Game duration |
| `created_at` | DATETIME | Game start time |
| `updated_at` | DATETIME | Last update |

**Indexes:**
- Primary: `id`
- Index: `format`
- Index: `winner_id`
- Index: `created_at`

---

#### `mtg_lc_saved_states`

Stores persistent game states.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | User ID (0 for anonymous) |
| `game_data` | LONGTEXT | JSON state |
| `share_token` | VARCHAR(64) | Share token (if shared) |
| `expires_at` | DATETIME | Expiration |
| `created_at` | DATETIME | Created |
| `updated_at` | DATETIME | Last update |

**Indexes:**
- Primary: `id`
- Unique: `share_token`
- Index: `user_id`
- Index: `expires_at`

---

#### `mtg_lc_presets`

User-saved presets.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | User ID |
| `name` | VARCHAR(255) | Preset name |
| `config_data` | LONGTEXT | JSON configuration |
| `created_at` | DATETIME | Created |

**Indexes:**
- Primary: `id`
- Index: `user_id`

---

### Analytics Tables

#### `mtg_lc_analytics_games`

Game-level analytics.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `game_id` | BIGINT(20) | References `mtg_lc_games.id` |
| `format` | VARCHAR(50) | Format |
| `player_count` | INT(11) | Players |
| `winner_id` | BIGINT(20) | Winner |
| `duration_minutes` | INT(11) | Duration |
| `game_date` | DATETIME | Date/time |
| `created_at` | DATETIME | Recorded |

**Indexes:**
- Primary: `id`
- Index: `game_id`
- Index: `format`
- Index: `game_date`

---

#### `mtg_lc_analytics_events`

Event-level analytics.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `game_id` | BIGINT(20) | Game ID |
| `event_type` | VARCHAR(50) | Event (life_change, dice_roll, etc.) |
| `event_data` | TEXT | JSON event data |
| `user_id` | BIGINT(20) | User ID |
| `ip_address` | VARCHAR(45) | IP (anonymized if enabled) |
| `created_at` | DATETIME | Timestamp |

**Indexes:**
- Primary: `id`
- Index: `game_id`
- Index: `event_type`
- Index: `created_at`

---

### Player Profile Tables

#### `mtg_lc_player_profiles`

Player profiles.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | WordPress user ID |
| `display_name` | VARCHAR(255) | Display name |
| `bio` | TEXT | Biography |
| `favorite_format` | VARCHAR(50) | Favorite format |
| `location` | VARCHAR(255) | Location |
| `avatar_url` | VARCHAR(500) | Avatar URL |
| `privacy` | VARCHAR(20) | public/friends/private |
| `total_games` | INT(11) | Games played |
| `total_wins` | INT(11) | Games won |
| `total_losses` | INT(11) | Games lost |
| `win_rate` | DECIMAL(5,2) | Win percentage |
| `created_at` | DATETIME | Profile created |
| `updated_at` | DATETIME | Last updated |

**Indexes:**
- Primary: `id`
- Unique: `user_id`
- Index: `display_name`

---

#### `mtg_lc_game_participants`

Links users to games.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `game_id` | BIGINT(20) | Game ID |
| `user_id` | BIGINT(20) | User ID |
| `won` | TINYINT(1) | 1 if won, 0 if lost |
| `created_at` | DATETIME | Recorded |

**Indexes:**
- Primary: `id`
- Index: `game_id`
- Index: `user_id`
- Index: `won`

---

#### `mtg_lc_achievements`

Achievement unlocks.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | User ID |
| `achievement_key` | VARCHAR(100) | Achievement identifier |
| `unlocked_at` | DATETIME | Unlock timestamp |

**Indexes:**
- Primary: `id`
- Unique: `user_id`, `achievement_key`
- Index: `user_id`

---

#### `mtg_lc_friends`

Friend relationships.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | User 1 |
| `friend_id` | BIGINT(20) | User 2 |
| `status` | VARCHAR(20) | pending/accepted |
| `created_at` | DATETIME | Request sent |
| `accepted_at` | DATETIME | Request accepted |

**Indexes:**
- Primary: `id`
- Index: `user_id`
- Index: `friend_id`
- Index: `status`

---

#### `mtg_lc_activity_feed`

Social activity feed.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | User ID |
| `activity_type` | VARCHAR(50) | Activity type |
| `activity_data` | TEXT | JSON data |
| `created_at` | DATETIME | Activity timestamp |

**Indexes:**
- Primary: `id`
- Index: `user_id`
- Index: `activity_type`
- Index: `created_at`

---

### Tournament Tables

#### `mtg_lc_tournaments`

Tournaments.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `name` | VARCHAR(255) | Tournament name |
| `slug` | VARCHAR(255) | URL slug |
| `type` | VARCHAR(50) | swiss/elimination/ladder/season |
| `format` | VARCHAR(50) | MTG format |
| `best_of` | INT(11) | 1 or 3 |
| `rounds` | INT(11) | Total rounds (Swiss) |
| `current_round` | INT(11) | Current round |
| `max_players` | INT(11) | Max enrollment |
| `status` | VARCHAR(20) | upcoming/active/completed |
| `start_date` | DATETIME | Start date/time |
| `end_date` | DATETIME | End date (for Season) |
| `entry_fee` | DECIMAL(10,2) | Entry fee |
| `prize_pool` | DECIMAL(10,2) | Prize pool |
| `description` | TEXT | Description |
| `created_by` | BIGINT(20) | Creator user ID |
| `created_at` | DATETIME | Created |
| `updated_at` | DATETIME | Updated |

**Indexes:**
- Primary: `id`
- Unique: `slug`
- Index: `type`
- Index: `status`

---

#### `mtg_lc_tournament_enrollments`

Player enrollments.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `tournament_id` | BIGINT(20) | Tournament ID |
| `user_id` | BIGINT(20) | User ID |
| `deck_name` | VARCHAR(255) | Deck name |
| `status` | VARCHAR(20) | enrolled/dropped |
| `enrolled_at` | DATETIME | Enrollment timestamp |
| `dropped_at` | DATETIME | Drop timestamp |

**Indexes:**
- Primary: `id`
- Unique: `tournament_id`, `user_id`
- Index: `tournament_id`
- Index: `user_id`

---

#### `mtg_lc_tournament_matches`

Match results.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `tournament_id` | BIGINT(20) | Tournament ID |
| `round` | INT(11) | Round number |
| `player1_id` | BIGINT(20) | Player 1 |
| `player2_id` | BIGINT(20) | Player 2 (NULL for bye) |
| `winner_id` | BIGINT(20) | Winner ID (NULL for draw/pending) |
| `player1_wins` | INT(11) | Games won by P1 |
| `player2_wins` | INT(11) | Games won by P2 |
| `result` | VARCHAR(20) | win/loss/draw |
| `reported_at` | DATETIME | Result reported |
| `created_at` | DATETIME | Match created |

**Indexes:**
- Primary: `id`
- Index: `tournament_id`
- Index: `round`
- Index: `player1_id`
- Index: `player2_id`

---

#### `mtg_lc_tournament_standings`

Cached standings (for performance).

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `tournament_id` | BIGINT(20) | Tournament ID |
| `user_id` | BIGINT(20) | User ID |
| `rank` | INT(11) | Current rank |
| `match_points` | INT(11) | Total points |
| `wins` | INT(11) | Wins |
| `losses` | INT(11) | Losses |
| `draws` | INT(11) | Draws |
| `omw_percent` | DECIMAL(5,2) | OMW% tiebreaker |
| `gw_percent` | DECIMAL(5,2) | Game Win% |
| `win_streak` | INT(11) | Current win streak |
| `updated_at` | DATETIME | Last calculated |

**Indexes:**
- Primary: `id`
- Unique: `tournament_id`, `user_id`
- Index: `tournament_id`
- Index: `rank`

---

### Deck Tables

#### `mtg_lc_decks`

User decks.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | Owner |
| `name` | VARCHAR(255) | Deck name |
| `format` | VARCHAR(50) | Format |
| `archetype` | VARCHAR(100) | Detected archetype |
| `deck_list` | LONGTEXT | JSON card list |
| `description` | TEXT | Description |
| `visibility` | VARCHAR(20) | public/private |
| `created_at` | DATETIME | Created |
| `updated_at` | DATETIME | Updated |

**Indexes:**
- Primary: `id`
- Index: `user_id`
- Index: `format`
- Index: `archetype`

---

### AI Tables

#### `mtg_lc_ai_insights`

Cached AI insights (to reduce API calls).

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | User ID |
| `insight_type` | VARCHAR(50) | Type (temporal/duration/format/etc.) |
| `insight_data` | TEXT | JSON insight |
| `generated_at` | DATETIME | Generated |
| `expires_at` | DATETIME | Cache expiration |

**Indexes:**
- Primary: `id`
- Index: `user_id`
- Index: `insight_type`
- Index: `expires_at`

---

#### `mtg_lc_ai_conversations`

Mentor chatbot history.

| Column | Type | Description |
|--------|------|-------------|
| `id` | BIGINT(20) | Primary key |
| `user_id` | BIGINT(20) | User ID |
| `message` | TEXT | User message |
| `response` | TEXT | AI response |
| `context_data` | TEXT | JSON context used |
| `created_at` | DATETIME | Message sent |

**Indexes:**
- Primary: `id`
- Index: `user_id`
- Index: `created_at`

---

## Troubleshooting

### Common Issues

#### Issue: Life counter not showing

**Symptoms:** Shortcode shows but no interface

**Causes:**
- JavaScript not loading
- Theme conflict
- Plugin conflict

**Solutions:**
1. Check browser console for errors
2. Temporarily switch to default WordPress theme
3. Deactivate other plugins one by one
4. Clear browser cache
5. Verify plugin is activated

---

#### Issue: Persistence not working

**Symptoms:** Games don't resume, save button doesn't work

**Solutions:**
1. Check **Settings → State Management**
2. Verify `persist="true"` in shortcode
3. Check browser localStorage is enabled
4. For server persistence: Check user is logged in
5. Check database tables exist
6. Verify AJAX endpoints working (check Network tab)

---

#### Issue: Tournament pairings not generating

**Symptoms:** "Start Round" button doesn't create pairings

**Solutions:**
1. Verify tournament status is "Active"
2. Check at least 2 players enrolled
3. For Swiss: Verify rounds calculated correctly
4. Check database permissions
5. Check error logs in WordPress debug mode
6. Ensure `mtg_lc_tournament_matches` table exists

---

#### Issue: AI features not working

**Symptoms:** Insights show "Error" or mentor doesn't respond

**Solutions:**
1. Verify API key is correct in **Settings → AI Insights**
2. Click "Test Connection" button
3. Check monthly API limit not exceeded
4. Verify user has played enough games (10+ for insights)
5. Check error logs for API response
6. Try switching AI provider
7. Disable AI and use SQL-only mode

---

#### Issue: Avatars not uploading

**Symptoms:** Upload fails or doesn't show

**Solutions:**
1. Check file size (max 5MB)
2. Verify file format (JPG, PNG, GIF only)
3. Check WordPress uploads directory permissions
4. Verify `upload_max_filesize` in php.ini
5. Check browser console for errors
6. Clear browser cache

---

#### Issue: Analytics not tracking

**Symptoms:** Dashboard shows no data

**Solutions:**
1. Verify **Settings → Analytics** is enabled
2. Play a complete game (start to finish)
3. Check logged in (for user tracking)
4. Verify `mtg_lc_analytics_games` table exists
5. Check AJAX endpoint `mtg_lc_analytics_track` working
6. Clear object cache if using caching plugin

---

### Debug Mode

Enable WordPress debug mode to see errors:

**wp-config.php:**
```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
```

Check logs at: `/wp-content/debug.log`

---

### Database Repair

If tables are corrupted:

1. Deactivate plugin
2. Go to **phpMyAdmin**
3. Select WordPress database
4. Find tables prefixed with `wp_mtg_lc_`
5. Select all → "Repair table"
6. Reactivate plugin

Or drop and recreate:
```sql
DROP TABLE wp_mtg_lc_games;
-- (repeat for all tables)
```
Then reactivate plugin to recreate.

---

### Performance Optimization

**For Large Databases:**

1. **Add Indexes:**
   - Already included on key columns
   - Add custom indexes if needed

2. **Object Caching:**
   - Install Redis or Memcached
   - Analytics auto-uses object cache

3. **Database Cleanup:**
   - Set retention period in **Settings → Analytics**
   - Manually delete old games
   - Run cleanup cron job

4. **Optimize Tables:**
   ```sql
   OPTIMIZE TABLE wp_mtg_lc_games;
   OPTIMIZE TABLE wp_mtg_lc_analytics_events;
   ```

---

### Support

**Getting Help:**

1. Check documentation (this file)
2. Review error logs
3. Search WordPress support forums
4. Contact plugin author

**Providing Debug Info:**

When reporting issues, include:
- WordPress version
- PHP version
- Plugin version
- Active theme
- Active plugins
- Error messages (from console/logs)
- Steps to reproduce

---

## Appendix

### Changelog Summary

**v4.3.1** - Current version
- Fixed avatar button click handling
- Improved mobile UI
- Bug fixes

**v4.0.0** - Tournament Mode
- Swiss, Elimination, Round Robin tournaments
- Ladder and Season modes
- 7 tournament shortcodes
- Admin tournament management

**v3.0.0** - AI Features
- AI Insights engine
- AI Mentor chatbot
- Claude, OpenAI, Hugging Face support

**v2.5.0** - Social Features
- Friends system
- Activity feed
- Challenges
- Head-to-head records

**v2.4.0** - Player Profiles
- Profile system
- Achievements
- Match history
- Leaderboards

**v2.2.0** - Analytics
- Analytics dashboard
- Chart.js integration
- CSV export

**v2.0.0** - State Management
- Game persistence
- Sharing features
- Spectator mode
- Presets

---

### Glossary

**EDH** - Elder Dragon Highlander, another name for Commander

**OMW%** - Opponent Match Win Percentage, a tiebreaker metric

**CMC** - Converted Mana Cost (now called Mana Value)

**Swiss** - Tournament format where players are paired based on standings

**Archetype** - Deck strategy/style (e.g., Aggro, Control)

**Bye** - Automatic win when odd number of players

**Best of 3** - Match format where first to win 2 games wins match

**Pairing** - Matched opponents for a round

**Standings** - Ranked list of tournament participants

---

### Credits

**Developer:** Sean Bailey - Mountain View Provisions LLC 
**Version:** 4.3.1  
**License:** Custom 


**Third-Party Libraries:**
- Chart.js - For analytics charts
- Scryfall API - For card search
- QR Code Generator - For share links

---

## Quick Reference Card

**Essential Shortcodes:**

```
Life Counter:     [mtg_life_counter]
Tournament:       [mtg_tournament slug="..."]
Profile:          [mtg_player_profile]
Login:            [mtg_login]
AI Insights:      [mtg_ai_insights]
Leaderboard:      [mtg_leaderboard]
```

**Admin Pages:**

```
Settings → MTG Life Counter
  ├─ General
  ├─ State Management
  ├─ Analytics
  ├─ AI Insights
  ├─ Player Profiles
  └─ Tournament
```

**Database Tables:** 14 total (prefix: `wp_mtg_lc_`)

**PHP Classes:** 24 classes

**JavaScript Files:** 16 files

**CSS Files:** 12 files

---

**End of Documentation**

*Last Updated: February 13, 2026*  
*Plugin Version: 4.3.1*
