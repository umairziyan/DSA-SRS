# LeetCode SRS

A simple CLI tool for spaced repetition learning of LeetCode problems. Never forget a problem you've solved.

## Why Spaced Repetition?

Research shows we forget 90% of new information within 7 days. Spaced repetition combats this by reviewing material at optimal intervals—just before you forget it. This tool uses the SM-2 algorithm (the same one behind Anki) adapted for coding problems.

## Installation

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/dsa-prep.git
cd dsa-prep

# Make it executable
chmod +x leetcode-srs

# Optional: Add to PATH
echo 'alias leetcode-srs="/path/to/dsa-prep/leetcode-srs"' >> ~/.bashrc
```

Requires Python 3.6+

## Usage

```bash
# Add a problem you just solved
./leetcode-srs add "Two Sum" "https://leetcode.com/problems/two-sum"

# Check what's due today
./leetcode-srs today

# Pull one of tomorrow's problems into today and review it now
./leetcode-srs pull "1. Two Sum"

# Push up to 3 due-new problems across the next few days
./leetcode-srs today --move-3

# Start a review session
./leetcode-srs review

# See all problems
./leetcode-srs list

# View statistics
./leetcode-srs stats
```

## Commands

| Command | Description |
|---------|-------------|
| `add <name> [url] [company]` | Add a new problem (optional company tag) |
| `today [--move-3]` | Show problems due for review today, optionally spreading up to 3 unreviewed new problems across the next days |
| `pull <name\|id>` | Review a specific problem from tomorrow early and reschedule it from today |
| `review` | Interactive review session |
| `list` | List all problems with status |
| `stats` | Show learning statistics |
| `delete <name\|id>` | Remove a problem |
| `tag <name\|id> <company> [--remove]` | Tag a problem with a company name |
| `reset <name\|id>` | Reset a problem's schedule to start fresh |
| `sync` | Manually sync to git |
| `help` | Show help message |

## Daily Goals & Load Management

The tool helps you maintain consistent practice:

- **Daily add goal**: Reminder to add at least 3 new problems per day
- **Priority system**: Top 5 most urgent problems are marked as `[!]` priority
- **Move 3 today**: Use `./leetcode-srs today --move-3` or press `m` from the `today` screen to spread up to 3 due-new problems across the next few days
- **Pull from tomorrow**: Use `./leetcode-srs pull "1. Two Sum"` to review one specific tomorrow problem early when the next day is overloaded
- **Smart scheduling**: New problems are auto-spread across days to prevent clustering
- **Overdue first**: Most overdue problems appear first in reviews

This prevents the common problem of review debt piling up while ensuring steady progress.

## The Algorithm

Based on the SM-2 spaced repetition algorithm with difficulty adjustments:

| Rating | Meaning | Effect |
|--------|---------|--------|
| **1 - Easy** | Knew it instantly | Interval × 2.5, ease +0.15 |
| **2 - Medium** | Some effort required | Interval × 2, ease unchanged |
| **3 - Hard** | Struggled but solved | Interval × 1.2, ease -0.20 |
| **0 - Failed** | Couldn't solve it | Reset to 1 day, ease -0.30 |

### Interval Progression Example

With "Medium" ratings: 1d → 2d → 4d → 8d → 16d → 32d → 64d → 128d → 256d

After ~8 successful reviews, a problem is deeply embedded in long-term memory.

## Git Sync

Your progress auto-syncs to git after every change:
- Commits with message `leetcode-srs: auto-sync <timestamp>`
- Pushes to remote (if configured)

This means you can:
- Track your learning progress over time
- Sync across multiple machines
- Never lose your data

To disable auto-sync, edit `GIT_SYNC_ENABLED = False` in the script.

## Data Storage

Data is stored in `leetcode-srs-data.json` in the same directory as the script. The JSON structure:

```json
{
  "problems": [
    {
      "id": "abc123",
      "name": "Two Sum",
      "url": "https://leetcode.com/problems/two-sum",
      "added_date": "2025-01-01",
      "next_review": "2025-01-02",
      "interval": 1,
      "ease_factor": 2.5,
      "review_count": 0,
      "last_difficulty": null,
      "history": []
    }
  ],
  "daily_adds": {
    "2025-01-01": 2
  }
}
```

## Company Tags

Label problems with company names for targeted interview prep:

```bash
# Add with tag inline
./leetcode-srs add "1. Two Sum" "https://leetcode.com/problems/two-sum" Meta

# Tag an existing problem
./leetcode-srs tag "two sum" Google

# Remove a tag
./leetcode-srs tag "two sum" Google --remove

# Duplicate add: prompts to tag + reset in one step
./leetcode-srs add "1. Two Sum" "url"
# → Enter "Amazon" at prompt → resets interval, adds tag
```

Tags are normalized globally: `meta`, `META`, and `Meta` all resolve to `Meta`. Major companies (Meta, Google, Amazon, Microsoft, etc.) get distinct colors in the terminal. Tags appear inline in `list`, `today`, and `review` output.

## Reset

Restart a problem's SRS schedule without losing its history:

```bash
./leetcode-srs reset "two sum"
```

Useful when you want to re-learn a problem from scratch for a new job search cycle. History and tags are preserved.

## Recommended Workflow

1. **Solve a problem** on LeetCode
2. **Add it**: `./leetcode-srs add "1. Problem Name" "url" Meta`
3. **Review daily**: Run `./leetcode-srs review` each day
4. **Pull ahead when needed**: Use `./leetcode-srs pull "1. Problem Name"` if tomorrow is too crowded and you have time now
5. **Rate honestly**: Be truthful about difficulty—it optimizes your schedule
6. **Trust the system**: Even if intervals feel long, that's the point

## Research Behind This

- [Ebbinghaus Forgetting Curve](https://en.wikipedia.org/wiki/Forgetting_curve) - Why we forget
- [SM-2 Algorithm](https://super-memory.com/english/ol/sm2.htm) - The original spaced repetition algorithm
- [FSRS Research](https://github.com/open-spaced-repetition/fsrs4anki) - Modern improvements

## License

MIT
