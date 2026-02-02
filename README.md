# Kosovo Prayer Times 2026

Official prayer times data for Kosovo for the year 2026, extracted from the Bashkësia Islame e Kosovës (Islamic Community of Kosovo) official Takvim.

## Data Source

- **Source**: Kryesia e Bashkësisë Islame e Republikës së Kosovës (BIK)
- **Original PDF**: [takvimi2026vaktet.pdf](https://dituriaislame.com/wp-content/uploads/2026/01/takvimi2026vaktet.pdf)
- **Hijri Year**: 1447-1448

## File Format

The `kosovo-prayer-times-2026.json` file contains:

### Metadata
- Source information and URL
- Location coordinates (42.5°N, 21.0°E - reference: Deçan)
- Calculation method details (BIM Kosovo method)
- Timezone information (CET/CEST)
- City-specific time offsets
- Major Islamic events for 2026

### Prayer Times
For each day of 2026:
- `date`: ISO format date (YYYY-MM-DD)
- `day_of_week`: English day name
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

## Usage Example

### JavaScript
```javascript
const data = require('./kosovo-prayer-times-2026.json');

// Get today's prayer times
const today = new Date().toISOString().split('T')[0];
const monthName = new Date().toLocaleString('en', { month: 'long' });
const day = new Date().getDate();

const todayPrayers = data.prayer_times[monthName].find(d => d.day === day);
console.log(`Fajr: ${todayPrayers.fajr}`);
console.log(`Maghrib: ${todayPrayers.maghrib}`);
```

### Python
```python
import json
from datetime import date

with open('kosovo-prayer-times-2026.json') as f:
    data = json.load(f)

today = date.today()
month_name = today.strftime('%B')
day = today.day

today_prayers = next(d for d in data['prayer_times'][month_name] if d['day'] == day)
print(f"Fajr: {today_prayers['fajr']}")
print(f"Maghrib: {today_prayers['maghrib']}")
```

### Swift
```swift
import Foundation

struct PrayerDay: Codable {
    let day: Int
    let date: String
    let dayOfWeek: String
    let imsak, fajr, sunrise, dhuhr, asr, maghrib, isha: String

    enum CodingKeys: String, CodingKey {
        case day, date, imsak, fajr, sunrise, dhuhr, asr, maghrib, isha
        case dayOfWeek = "day_of_week"
    }
}
```

## Calculation Method

The BIM Kosovo calculation method uses:
- **Fajr angle**: 18°
- **Isha angle**: 17°
- **Temkin**: 1.5° (6 minutes)
- **Fajr offset from Imsak**: 20 minutes

## Islamic Events 2026

| Event | Date |
|-------|------|
| Laylat al-Miraj | January 15 |
| Laylat al-Bara'at | February 2 |
| Ramadan Start | February 19 |
| Laylat al-Qadr | March 16 |
| Eid al-Fitr | March 20 |
| Eid al-Adha | May 27 |
| Islamic New Year 1448 | June 16 |
| Ashura | June 25 |
| Mawlid | August 24 |

## Daylight Saving Time

- **DST Start**: March 29, 2026 at 02:00 (+1 hour)
- **DST End**: October 25, 2026 at 03:00 (-1 hour)

Times in the JSON already account for DST changes.

## License

This data is extracted from publicly available official religious calendar published by BIK for the benefit of the Muslim community. Please attribute the source when using this data.

## Contributing

If you find any errors in the data, please open an issue with:
- The specific date(s) affected
- The expected vs actual values
- Reference to the original PDF page

## Related Projects

- [Takvimi-per-Evrope-per-MacOS](https://github.com/drilon/Takvimi-per-Evrope-per-MacOS) - macOS prayer times app
- [Takvimi-per-Evrope-Windows](https://github.com/drilon/Takvimi-per-Evrope-Windows) - Windows prayer times app
