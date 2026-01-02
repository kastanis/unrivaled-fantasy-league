# Converting Unrivaled Box Scores to CSV

## Quick Reference Guide

### From Unrivaled Box Score → Your CSV Format

| Unrivaled Column | Your CSV Column | Example |
|-----------------|-----------------|---------|
| **PTS** | **PTS** = same value | 21 |
| **REB** | **REB** = same value | 9 |
| **AST** | **AST** = same value | 1 |
| **STL** | **STL** = same value | 1 |
| **BLK** | **BLK** = same value | 0 |
| **TO** | **TOV** = same value | 3 |
| **PF** | **PF** = same value | 3 |
| Special notes | **GAME_WINNER** = 1 if noted, else 0 | Check "Game Winner" section |
| Play-by-play | **DUNK** = count from video/notes | Usually 0-2 per player |

## Step-by-Step Process

### Example: Kahleah Copper's Line
**Box Score Shows:**
- PTS: 21
- REB: 9, AST: 1, STL: 1, BLK: 0, TO: 3, PF: 3

**Your CSV Entry:**
```
PTS = 21
REB = 9
AST = 1
STL = 1
BLK = 0
TOV = 3
PF = 3
GAME_WINNER = 0    (game winner was Azurá Stevens, not Copper)
DUNK = 0           (check video if unsure)
```

## Finding Player IDs

Use the [data/handmade/players.csv](data/handmade/players.csv) file to match player names to IDs:

```csv
player_id,player_name,team,status
5,Kahleah Copper,Rose,active
2,Rickea Jackson,Mist,active
...
```

## Game Winner Detection

Look for the "Game Winner" note in the box score:
- Example: "Azurá Stevens—Two point shot"
- Give that player `GAME_WINNER = 1`
- All others get `GAME_WINNER = 0`

## Dunk Tracking

Dunks are NOT in the standard box score. You'll need to:
1. Watch the game highlights
2. Check the play-by-play if available
3. Or just set to 0 if you're not tracking dunks

Most games will have 0-3 total dunks across all players.

## Common Mistakes to Avoid

❌ **Wrong:** Mixing up TOV (turnovers) and TO column name
```
Box score shows TO: 3  →  TO = 3  ❌ NO!
```

✅ **Right:** Use TOV as the column name
```
Box score shows TO: 3  →  TOV = 3  ✓ YES!
```

## Template CSV

```csv
game_id,player_id,PTS,REB,AST,STL,BLK,TOV,PF,GAME_WINNER,DUNK
1,5,21,9,1,1,0,3,3,0,0
```

## Time-Saving Tips

1. **Copy box score to spreadsheet** - Makes entry faster
2. **Use Excel template** - See [EXCEL_CONVERSION_SETUP.md](EXCEL_CONVERSION_SETUP.md)
3. **Batch process** - Do all players from one team, then the other
4. **Double-check totals** - Your PTS should match the box score

## Verification

After creating CSV, verify one player manually:

**Kahleah Copper:**
- Game Points (from box score): **21** ✓
- Fantasy Points: 21×1.0 + 9×1.2 + 1×1.0 + 1×2.0 + 0×2.0 + 3×(-1.0) + 3×(-0.5) = **29.8 pts**

Game points in your CSV should match box score (21 = 21 ✓)
