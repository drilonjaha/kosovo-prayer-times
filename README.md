# Kosovo Prayer Times

Official prayer times data for Kosovo, extracted from the Bashkësia Islame e Kosovës (Islamic Community of Kosovo) official Takvim.

## Important: Prayer Times Are Year-Independent

**Prayer times for any given calendar date (e.g., February 2nd) are identical every year.** This is because prayer times are calculated based on the sun's position, which depends only on:

- Geographic coordinates (latitude/longitude)
- Day of the year (1-365)

The sun is in the exact same position on February 2nd, 2025 as it is on February 2nd, 2026, 2027, or any other year.

### What changes between years:
- **Day of the week** - shifts by 1 day each year (2 in leap years)
- **Islamic (Hijri) calendar dates** - shift ~11 days earlier each year
- **Islamic holidays** - fall on different Gregorian dates

### Verified Comparison (February 2nd)
| Prayer | 2025 | 2026 | Same? |
|--------|------|------|-------|
| Fajr | 5:29 | 5:29 | ✓ |
| Dhuhr | 11:54 | 11:54 | ✓ |
| Asr | 14:35 | 14:35 | ✓ |
| Maghrib | 16:58 | 16:58 | ✓ |
| Isha | 18:31 | 18:31 | ✓ |

**This means you can use this data for any year** - just update the day-of-week mapping and Islamic calendar events as needed.

## Data Source

- **Source**: Kryesia e Bashkësisë Islame e Republikës së Kosovës (BIK)
- **Original PDF**: [takvimi2026vaktet.pdf](https://dituriaislame.com/wp-content/uploads/2026/01/takvimi2026vaktet.pdf)
- **Reference Hijri Year**: 1447-1448

## File Formats

| File | Size | Description |
|------|------|-------------|
| `kosovo-prayer-times.json` | 114 KB | Full JSON with metadata |
| `kosovo-prayer-times.min.json` | 69 KB | Minified for production |
| `kosovo-prayer-times.csv` | 24 KB | Flat CSV for spreadsheets |

### JSON Structure
```json
{
  "metadata": {
    "source": "BIK",
    "location": { "latitude": 42.5, "longitude": 21.0 },
    "calculation_method": { "fajr_angle": 18, "isha_angle": 17 },
    "city_offsets_minutes": { "Prishtina": -1, ... }
  },
  "prayer_times": {
    "January": [
      { "day": 1, "fajr": "5:41", "dhuhr": "11:44", "maghrib": "16:21", ... }
    ]
  }
}
```

### Prayer Times Fields
- `imsak`: Pre-dawn (start of fasting)
- `fajr`: Dawn prayer
- `sunrise`: Sunrise time
- `dhuhr`: Noon prayer
- `asr`: Afternoon prayer
- `maghrib`: Sunset prayer
- `isha`: Night prayer
- `day_length`: Duration of daylight

## City Adjustments

Apply these offsets to the base times for different cities:

| City | Offset |
|------|--------|
| Sharri | +2 min |
| Ferizaj | -1 min |
| Gjilan | -1 min |
| Prishtina | -1 min |
| Podujeva | -1 min |
| Vushtrri | -1 min |
| Presheva | -2 min |

## Usage Examples

### JavaScript
```javascript
const data = require('./kosovo-prayer-times.json');

// Get prayer times for any date (works for any year!)
function getPrayerTimes(month, day) {
  return data.prayer_times[month].find(d => d.day === day);
}

const today = getPrayerTimes('February', 2);
console.log(`Fajr: ${today.fajr}`);    // 5:29
console.log(`Maghrib: ${today.maghrib}`); // 16:58
```

### Python
```python
import json

with open('kosovo-prayer-times.json') as f:
    data = json.load(f)

def get_prayer_times(month: str, day: int) -> dict:
    """Get prayer times for any date (works for any year!)"""
    return next(d for d in data['prayer_times'][month] if d['day'] == day)

today = get_prayer_times('February', 2)
print(f"Fajr: {today['fajr']}")      # 5:29
print(f"Maghrib: {today['maghrib']}") # 16:58
```

### Swift
```swift
struct PrayerDay: Codable {
    let day: Int
    let imsak, fajr, sunrise, dhuhr, asr, maghrib, isha: String

    enum CodingKeys: String, CodingKey {
        case day, imsak, fajr, sunrise, dhuhr, asr, maghrib, isha
    }
}
```

## Calculation Method

The BIM Kosovo calculation method uses:
- **Fajr angle**: 18°
- **Isha angle**: 17°
- **Temkin**: 1.5° (6 minutes)
- **Fajr offset from Imsak**: 20 minutes
- **Reference point**: Deçan (westernmost point of Kosovo)

## Daylight Saving Time

Times in the JSON already account for DST. Kosovo observes:
- **DST Start**: Last Sunday of March at 02:00 (+1 hour)
- **DST End**: Last Sunday of October at 03:00 (-1 hour)

## Islamic Events (2026 Reference)

| Event | 2026 Date |
|-------|-----------|
| Laylat al-Miraj | January 15 |
| Laylat al-Bara'at | February 2 |
| Ramadan Start | February 19 |
| Laylat al-Qadr | March 16 |
| Eid al-Fitr | March 20 |
| Eid al-Adha | May 27 |
| Islamic New Year 1448 | June 16 |
| Ashura | June 25 |
| Mawlid | August 24 |

*Note: Islamic dates shift ~11 days earlier each Gregorian year.*

## License

MIT License - See [LICENSE](LICENSE) file.

Data sourced from the official BIK Takvim. Please attribute when using.

## Contributing

Found an error? Open an issue with:
- The specific date(s) affected
- Expected vs actual values
- Reference to the original PDF

## Related Projects

- [Takvimi-per-Evrope-per-MacOS](https://github.com/drilonjaha/Takvimi-per-Evrope-per-MacOS) - macOS prayer times app
- [Takvimi-per-Evrope-Windows](https://github.com/drilonjaha/Takvimi-per-Evrope-Windows) - Windows prayer times app
