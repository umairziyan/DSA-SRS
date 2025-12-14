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
| `add <name> [url]` | Add a new problem (first review tomorrow) |
| `today` | Show problems due for review today |
| `review` | Interactive review session |
| `list` | List all problems with status |
| `stats` | Show learning statistics |
| `delete <name\|id>` | Remove a problem |
| `sync` | Manually sync to git |
| `help` | Show help message |

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
  ]
}
```

## Recommended Workflow

1. **Solve a problem** on LeetCode
2. **Add it**: `./leetcode-srs add "Problem Name" "url"`
3. **Review daily**: Run `./leetcode-srs review` each day
4. **Rate honestly**: Be truthful about difficulty—it optimizes your schedule
5. **Trust the system**: Even if intervals feel long, that's the point

## Research Behind This

- [Ebbinghaus Forgetting Curve](https://en.wikipedia.org/wiki/Forgetting_curve) - Why we forget
- [SM-2 Algorithm](https://super-memory.com/english/ol/sm2.htm) - The original spaced repetition algorithm
- [FSRS Research](https://github.com/open-spaced-repetition/fsrs4anki) - Modern improvements

## License

MIT
