# キックベース分析 (Kickbase Analytics)

A single-file web app for recording and analyzing kickball plays. Track ball speed, landing positions, and out types per player — no server required.

## Features

- **Play recording** — Click the field to mark a landing point, select ball speed (1–5), out type, and the player who kicked.
- **Analytics dashboard** — View landing point distributions on the field, out type breakdown (donut chart), and per-member out rates (bar chart).
- **Team & member management** — Add multiple teams and manage rosters from the settings page.
- **Export / Import** — Persist data across devices by exporting to JSON and importing it back.
- **Offline-first** — All data is stored in `localStorage`. No network connection needed after loading the page.
- **Dark mode** — Automatically adapts to the system color scheme.

## Usage

Open `index.html` in any modern browser. No build step or dependencies required.

```
open index.html
```

### Recording a play

1. Go to **記録 (Record)**.
2. Select a team and the player who kicked.
3. Click the field diagram to set the landing point.
4. Choose ball speed (1 = very slow, 5 = very fast) and out type if applicable.
5. Click **記録する** to save.

### Viewing analytics

Go to **分析 (Analytics)** to see:

- Aggregate stats: total plays, out count, out rate, average speed
- Field scatter plot of all landing points (green = safe, red = out)
- Donut chart of out types (fly, force, tag, foul)
- Bar chart of out rate per member

Filter by team using the tab bar at the top.

### Managing teams

Go to **設定 (Settings)** to add/remove teams and members. Deleting a team also removes all associated play records.

## Data format

Data is stored under the `kickbase-analytics-v1` key in `localStorage` and exported as JSON with the following shape:

```json
{
  "teams": [
    {
      "id": "team-abc",
      "name": "レッドソックス",
      "members": [
        { "id": "m-xyz", "name": "田中" }
      ]
    }
  ],
  "plays": [
    {
      "id": "play-abc",
      "teamId": "team-abc",
      "memberId": "m-xyz",
      "speed": 3,
      "x": 0.72,
      "y": 0.65,
      "outed": true,
      "outType": "fly",
      "createdAt": 1718000000000
    }
  ]
}
```

`x` and `y` are normalized coordinates (0–1) relative to the field width and height. `outType` is one of `fly`, `force`, `tag`, or `foul`; `null` when `outed` is `false`.

## Field layout

The field grid is 6×5. The infield diamond occupies the bottom-right quadrant (roughly 56–88% on both axes). The top strip and left strip represent the extra zone (out-of-bounds area).

## Browser support

Any evergreen browser (Chrome, Firefox, Safari, Edge). Uses `localStorage`, CSS `oklch()`, and `backdrop-filter`.

## License

MIT
