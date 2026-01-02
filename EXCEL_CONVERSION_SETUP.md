# Excel Template for Stats Conversion

## Setup Instructions

### Step 1: Create Excel Template

Open Excel/Google Sheets and create these columns:

| Column | Header | Description | Formula? |
|--------|--------|-------------|----------|
| A | Player Name | Type from box score | Manual |
| B | Team | Rose/Mist/etc | Manual |
| C | PTS | Total points scored | Manual |
| D | REB | Total rebounds | Manual |
| E | AST | Assists | Manual |
| F | STL | Steals | Manual |
| G | BLK | Blocks | Manual |
| H | TO | Turnovers | Manual |
| I | PF | Personal fouls | Manual |
| J | Winner? | YES or NO | Manual |
| K | Dunks | Count (usually 0) | Manual |
| **L** | **player_id** | **Look up from players.csv** | **Use INDEX/MATCH** |
| **M** | **PTS** | **=C2** | **Formula** |
| **N** | **REB** | **=D2** | **Formula** |
| **O** | **AST** | **=E2** | **Formula** |
| **P** | **STL** | **=F2** | **Formula** |
| **Q** | **BLK** | **=G2** | **Formula** |
| **R** | **TOV** | **=H2** | **Formula** |
| **S** | **PF** | **=I2** | **Formula** |
| **T** | **GAME_WINNER** | **=IF(J2="YES",1,0)** | **Formula** |
| **U** | **DUNK** | **=K2** | **Formula** |

### Step 2: Add Formulas (Starting in Row 2)

**Cell L2** (player_id lookup):
```excel
=INDEX(PlayerList!A:A,MATCH(A2,PlayerList!B:B,0))
```

**Cells M2-S2** (Copy stats):
```excel
M2: =C2  (PTS)
N2: =D2  (REB)
O2: =E2  (AST)
P2: =F2  (STL)
Q2: =G2  (BLK)
R2: =H2  (TOV)
S2: =I2  (PF)
```

**Cell T2** (GAME_WINNER):
```excel
=IF(J2="YES",1,0)
```

**Cell U2** (DUNK):
```excel
=K2
```

### Step 3: Create Player Lookup Sheet

Create a second sheet named "PlayerList" with:
- Column A: Player IDs (1-48)
- Column B: Player names (copy from `data/handmade/players.csv`)

Example:
```
A    B
1    Paige Bueckers
2    Rickea Jackson
3    Dominique Malonga
...
```

### Step 4: Usage Workflow

**For each game:**

1. Open Unrivaled box score in browser
2. Copy stats into Excel columns A-K (the manual columns)
3. Formulas in columns L-U calculate automatically
4. Add game_id column manually (set to 1 for first game, 2 for second, etc.)
5. Copy columns with these headers: `game_id,player_id,PTS,REB,AST,STL,BLK,TOV,PF,GAME_WINNER,DUNK`
6. Paste into new CSV file
7. Upload to Admin Portal

## Multiple Games Same Day

**Option 1: Separate CSVs**
- Create one CSV per game
- Upload each one separately with different game numbers

**Option 2: Combined CSV** (Recommended)
- Combine all games from same day in one Excel file
- Change game_id for each game (game 1 = 1, game 2 = 2, etc.)
- Export entire sheet as one CSV
- Upload once with the game date

Example combined CSV:
```csv
game_id,player_id,PTS,REB,AST,STL,BLK,TOV,PF,GAME_WINNER,DUNK
1,5,21,9,1,1,0,3,3,0,0
1,2,15,7,1,0,0,1,1,0,0
2,10,18,4,7,0,0,3,3,1,0
2,15,12,8,2,1,1,2,4,0,0
```

## Quick Copy Template

For Google Sheets users, here's a template you can copy:

```
=ARRAYFORMULA(IF(ROW(A:A)=1,"player_id",IF(A:A="","",INDEX(PlayerList!A:A,MATCH(A:A,PlayerList!B:B,0)))))
```

This auto-fills the player_id column based on player names.

## Tips

1. **Save your template** - Keep it open and reuse for each game day
2. **Color code** - Make manual entry columns yellow, formula columns green
3. **Double-check** - Verify PTS matches the box score
4. **Batch process** - Do all games for the week on Sunday night
5. **Use filters** - Excel filters help find specific players quickly

## Example Row

If box score shows:
- Kahleah Copper
- PTS: 21, REB: 9, AST: 1, STL: 1, BLK: 0, TO: 3, PF: 3

You enter in columns A-K:
```
A: Kahleah Copper
B: Rose
C: 21
D: 9
E: 1
F: 1
G: 0
H: 3
I: 3
J: NO
K: 0
```

Excel calculates in columns L-U:
```
L: 5 (her player_id)
M: 21 (PTS)
N: 9 (REB)
O: 1 (AST)
P: 1 (STL)
Q: 0 (BLK)
R: 3 (TOV)
S: 3 (PF)
T: 0 (not game winner)
U: 0 (no dunks)
```

Final CSV line:
```csv
1,5,21,9,1,1,0,3,3,0,0
```
