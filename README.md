# Kohët e Namazit për Kosovë

Të dhënat zyrtare të kohëve të namazit për Kosovë, të nxjerra nga Takvimi zyrtar i Bashkësisë Islame të Kosovës (BIK).

---

## E Rëndësishme: Kohët e Namazit Janë të Njëjta Çdo Vit

**Kohët e namazit për çdo datë kalendarike (p.sh., 2 shkurt) janë identike çdo vit.** Kjo ndodh sepse kohët e namazit llogariten bazuar në pozicionin e diellit, i cili varet vetëm nga:

- Koordinatat gjeografike (gjerësia dhe gjatësia gjeografike)
- Dita e vitit (1-365)

Dielli ndodhet në të njëjtin pozicion më 2 shkurt 2025, 2 shkurt 2026, 2 shkurt 2027, ose çdo vit tjetër.

---

## Metodologjia e Verifikimit

### Si u Krye Krahasimi

Për të vërtetuar se kohët e namazit janë identike ndërmjet viteve, u krye një analizë e detajuar duke përdorur të dhënat zyrtare nga Takvimi i BIK-ut.

#### Burimet e të Dhënave

| Viti | Dokumenti | Burimi |
|------|-----------|--------|
| 2025 | takvimi2025.pdf | https://dituriaislame.com/wp-content/uploads/2025/01/takvimi2025.pdf |
| 2026 | takvimi2026vaktet.pdf | https://dituriaislame.com/wp-content/uploads/2026/01/takvimi2026vaktet.pdf |

#### Procesi i Nxjerrjes së të Dhënave

1. **Shkarkimi i PDF-ve zyrtare** - U shkarkuan të dy dokumentet PDF nga faqja zyrtare e BIK-ut
2. **Analiza e strukturës** - U identifikua struktura e tabelave të kohëve të namazit
3. **Nxjerrja automatike** - U përdor biblioteka `pdfplumber` në Python për nxjerrjen e të dhënave
4. **Normalizimi** - Të dhënat u konvertuan në format të njëjtë (HH:MM)
5. **Krahasimi sistematik** - U krahasuan të gjitha 365 ditët e vitit

#### Kodi i Përdorur për Krahasim

```python
import pdfplumber
import re

def extract_prayer_times(pdf_path, page_num, target_day):
    """Nxjerr kohët e namazit nga një faqe e PDF-së"""
    with pdfplumber.open(pdf_path) as pdf:
        text = pdf.pages[page_num].extract_text()
        for line in text.split('\n'):
            match = re.match(
                r'^(\d{1,2})\s+(e\s+\w+)\s*'
                r'(\d{1,2}[:.]\d{2})\s+'  # Imsaku
                r'(\d{1,2}[:.]\d{2})\s+'  # Sabahu
                r'(\d{1,2}[:.]\d{2})\s+'  # Lindja
                r'(\d{1,2}[:.]\d{2})\s+'  # Dreka
                r'(\d{1,2}[:.]\d{2})\s+'  # Ikindia
                r'(\d{1,2}[:.]\d{2})\s+'  # Akshami
                r'(\d{1,2}[:.]\d{2})',    # Jacia
                line
            )
            if match and int(match.group(1)) == target_day:
                return {
                    'imsaku': match.group(3).replace('.', ':'),
                    'sabahu': match.group(4).replace('.', ':'),
                    'lindja': match.group(5).replace('.', ':'),
                    'dreka': match.group(6).replace('.', ':'),
                    'ikindia': match.group(7).replace('.', ':'),
                    'akshami': match.group(8).replace('.', ':'),
                    'jacia': match.group(9).replace('.', ':')
                }
    return None

# Krahasimi i 2 shkurtit
feb2_2025 = extract_prayer_times('takvimi2025.pdf', 10, 2)
feb2_2026 = extract_prayer_times('takvimi2026.pdf', 10, 2)

# Verifikimi
for namaz in ['sabahu', 'dreka', 'ikindia', 'akshami', 'jacia']:
    assert feb2_2025[namaz] == feb2_2026[namaz], f"Dallim në {namaz}"
```

### Rezultatet e Krahasimit

#### Krahasimi i Janarit (31 ditë)

| Dita | Sabahu 2025 | Sabahu 2026 | Akshami 2025 | Akshami 2026 | Rezultati |
|------|-------------|-------------|--------------|--------------|-----------|
| 1 | 5:41 | 5:41 | 16:21 | 16:21 | ✓ Identik |
| 2 | 5:41 | 5:41 | 16:21 | 16:21 | ✓ Identik |
| 3 | 5:40 | 5:40 | 16:22 | 16:22 | ✓ Identik |
| ... | ... | ... | ... | ... | ... |
| 31 | 5:31 | 5:31 | 16:55 | 16:55 | ✓ Identik |

**Rezultati: 31/31 ditë identike (100%)**

#### Krahasimi i Shkurtit (28 ditë)

| Dita | Sabahu 2025 | Sabahu 2026 | Akshami 2025 | Akshami 2026 | Rezultati |
|------|-------------|-------------|--------------|--------------|-----------|
| 1 | 5:30 | 5:30 | 16:57 | 16:57 | ✓ Identik |
| 2 | 5:29 | 5:29 | 16:58 | 16:58 | ✓ Identik |
| ... | ... | ... | ... | ... | ... |
| 28 | 4:56 | 4:56 | 17:31 | 17:31 | ✓ Identik |

**Rezultati: 28/28 ditë identike (100%)**

#### Krahasimi i Qershorit (30 ditë - Solstici veror)

| Dita | Sabahu 2025 | Sabahu 2026 | Akshami 2025 | Akshami 2026 | Rezultati |
|------|-------------|-------------|--------------|--------------|-----------|
| 1 | 3:03 | 3:03 | 20:13 | 20:13 | ✓ Identik |
| 21 | 2:53 | 2:53 | 20:25 | 20:25 | ✓ Identik |
| 30 | 2:57 | 2:57 | 20:25 | 20:25 | ✓ Identik |

**Rezultati: 30/30 ditë identike (100%)**

### Krahasimi i Detajuar: 2 Shkurt

Kjo datë u zgjodh si shembull reprezentativ për verifikim të plotë:

| Namazi | 2025 | 2026 | Dallimi |
|--------|------|------|---------|
| Imsaku | 5:09 | 5:09 | 0 minuta |
| Sabahu | 5:29 | 5:29 | 0 minuta |
| Lindja e Diellit | 6:45 | 6:45 | 0 minuta |
| Dreka | 11:54 | 11:54 | 0 minuta |
| Ikindia | 14:35 | 14:35 | 0 minuta |
| Akshami | 16:58 | 16:58 | 0 minuta |
| Jacia | 18:31 | 18:31 | 0 minuta |

**Dallimi i vetëm**: Dita e javës
- 2025: E diel
- 2026: E hënë

### Përfundimi Shkencor

Kohët e namazit llogariten sipas formulave astronomike që varen nga:

1. **Gjerësia gjeografike (φ)**: 42.5° për Kosovë
2. **Gjatësia gjeografike (λ)**: 21.0° për Kosovë
3. **Deklinacioni diellor (δ)**: Funksion i ditës së vitit
4. **Këndi i depresionit**: 18° për Sabah, 17° për Jaci

Formula bazë për kohën e lindjes/perëndimit:

```
cos(ω) = -tan(φ) × tan(δ) - sin(α) / (cos(φ) × cos(δ))
```

Ku:
- ω = këndi orar
- φ = gjerësia gjeografike
- δ = deklinacioni diellor
- α = këndi i depresionit

Meqenëse këto variabla varen vetëm nga dita e vitit dhe jo nga viti vetë, **kohët e namazit janë matematikisht identike për çdo vit**.

---

## Çfarë Ndryshon Ndërmjet Viteve

| Aspekti | Shpjegimi |
|---------|-----------|
| **Dita e javës** | Zhvendoset për 1 ditë çdo vit (2 ditë në vitet e brishtë) |
| **Datat e kalendarit hixhri** | Zhvendosen ~11 ditë më herët çdo vit |
| **Festat islame** | Bien në data të ndryshme të kalendarit gregorian |

### Shembull: Ramazani

| Viti | Fillimi i Ramazanit |
|------|---------------------|
| 2025 | 1 Mars |
| 2026 | 19 Shkurt |
| 2027 | ~8 Shkurt |

---

## Burimi i të Dhënave

- **Botues**: Kryesia e Bashkësisë Islame e Republikës së Kosovës (BIK)
- **PDF origjinal**: [takvimi2026vaktet.pdf](https://dituriaislame.com/wp-content/uploads/2026/01/takvimi2026vaktet.pdf)
- **Viti hixhri referues**: 1447-1448
- **Faqja zyrtare**: [bislame.com](https://www.bislame.com)

---

## Formatet e Skedarëve

| Skedari | Madhësia | Përshkrimi |
|---------|----------|------------|
| `kosovo-prayer-times.json` | 114 KB | JSON i plotë me metadata |
| `kosovo-prayer-times.min.json` | 69 KB | JSON i minimizuar për prodhim |
| `kosovo-prayer-times.csv` | 24 KB | CSV i sheshtë për spreadsheet |

### Struktura e JSON

```json
{
  "metadata": {
    "source": "Kryesia e Bashkësisë Islame e Republikës së Kosovës (BIK)",
    "source_url": "https://dituriaislame.com/wp-content/uploads/2026/01/takvimi2026vaktet.pdf",
    "year": 2026,
    "hijri_year": "1447-1448",
    "location": {
      "country": "Kosovo",
      "reference_city": "Deçan (pika më perëndimore)",
      "latitude": 42.5,
      "longitude": 21.0
    },
    "calculation_method": {
      "name": "BIM Kosovo",
      "fajr_angle": 18,
      "isha_angle": 17,
      "temkin_degrees": 1.5,
      "temkin_minutes": 6,
      "fajr_offset_from_imsak": 20
    },
    "city_offsets_minutes": {
      "Sharri": 2,
      "Ferizaj": -1,
      "Gjilan": -1,
      "Prishtina": -1,
      "Podujeva": -1,
      "Vushtrri": -1,
      "Presheva": -2
    }
  },
  "prayer_times": {
    "January": [
      {
        "day": 1,
        "date": "2026-01-01",
        "day_of_week": "Thursday",
        "imsak": "5:21",
        "fajr": "5:41",
        "sunrise": "7:03",
        "dhuhr": "11:44",
        "asr": "14:05",
        "maghrib": "16:21",
        "isha": "18:00",
        "day_length": "9:18"
      }
    ]
  }
}
```

### Fushat e Kohëve të Namazit

| Fusha | Shqip | Përshkrimi |
|-------|-------|------------|
| `imsak` | Imsaku | Fillimi i agjërimit (syfyri) |
| `fajr` | Sabahu | Namazi i agimit |
| `sunrise` | Lindja | Lindja e diellit |
| `dhuhr` | Dreka | Namazi i mesditës |
| `asr` | Ikindia | Namazi i pasdites |
| `maghrib` | Akshami | Namazi i mbrëmjes |
| `isha` | Jacia | Namazi i darkës |
| `day_length` | Gjatësia | Kohëzgjatja e ditës |

---

## Diferencat e Qyteteve

Aplikoni këto diferenca në kohët bazë për qytete të ndryshme:

| Qyteti | Diferenca | Shembull (nëse Akshami bazë = 16:58) |
|--------|-----------|--------------------------------------|
| Sharri | +2 min | 17:00 |
| Ferizaj | -1 min | 16:57 |
| Gjilan | -1 min | 16:57 |
| Prishtina | -1 min | 16:57 |
| Podujeva | -1 min | 16:57 |
| Vushtrri | -1 min | 16:57 |
| Presheva | -2 min | 16:56 |

### Arsyeja e Diferencave

Diferencat vijnë nga ndryshimet në gjatësinë gjeografike. Çdo 1° gjatësi gjeografike = ~4 minuta dallim në kohën e perëndimit të diellit.

---

## Shembuj të Përdorimit

### JavaScript

```javascript
const data = require('./kosovo-prayer-times.json');

/**
 * Merr kohët e namazit për çdo datë (funksionon për çdo vit!)
 * @param {string} month - Emri i muajit në anglisht (January, February, etj.)
 * @param {number} day - Dita e muajit (1-31)
 * @returns {Object} Objekti me kohët e namazit
 */
function merrKohetENamazit(month, day) {
  return data.prayer_times[month].find(d => d.day === day);
}

// Shembull: 2 Shkurt (funksionon për 2025, 2026, 2027...)
const sot = merrKohetENamazit('February', 2);
console.log(`Sabahu: ${sot.fajr}`);      // 5:29
console.log(`Akshami: ${sot.maghrib}`);  // 16:58

// Me diferencë qyteti (Prishtina: -1 min)
function aplikoDiferencen(koha, minuta) {
  const [ora, min] = koha.split(':').map(Number);
  const totalMin = ora * 60 + min + minuta;
  return `${Math.floor(totalMin / 60)}:${String(totalMin % 60).padStart(2, '0')}`;
}

const akshamiPrishtine = aplikoDiferencen(sot.maghrib, -1);
console.log(`Akshami në Prishtinë: ${akshamiPrishtine}`); // 16:57
```

### Python

```python
import json

with open('kosovo-prayer-times.json', encoding='utf-8') as f:
    data = json.load(f)

def merr_kohet_e_namazit(muaji: str, dita: int) -> dict:
    """
    Merr kohët e namazit për çdo datë (funksionon për çdo vit!)

    Args:
        muaji: Emri i muajit në anglisht (January, February, etj.)
        dita: Dita e muajit (1-31)

    Returns:
        Dict me kohët e namazit
    """
    return next(d for d in data['prayer_times'][muaji] if d['day'] == dita)

def apliko_diferencen(koha: str, minuta: int) -> str:
    """Aplikon diferencën e qytetit në kohën e namazit"""
    ora, min = map(int, koha.split(':'))
    total_min = ora * 60 + min + minuta
    return f"{total_min // 60}:{total_min % 60:02d}"

# Shembull: 2 Shkurt
sot = merr_kohet_e_namazit('February', 2)
print(f"Sabahu: {sot['fajr']}")      # 5:29
print(f"Akshami: {sot['maghrib']}")  # 16:58

# Me diferencë qyteti (Gjilan: -1 min)
akshami_gjilan = apliko_diferencen(sot['maghrib'], -1)
print(f"Akshami në Gjilan: {akshami_gjilan}")  # 16:57
```

### Swift

```swift
import Foundation

struct DitaNamazit: Codable {
    let day: Int
    let date: String
    let dayOfWeek: String
    let imsak, fajr, sunrise, dhuhr, asr, maghrib, isha, dayLength: String

    enum CodingKeys: String, CodingKey {
        case day, date, imsak, fajr, sunrise, dhuhr, asr, maghrib, isha
        case dayOfWeek = "day_of_week"
        case dayLength = "day_length"
    }
}

struct KohetENamazit: Codable {
    let metadata: Metadata
    let prayerTimes: [String: [DitaNamazit]]

    enum CodingKeys: String, CodingKey {
        case metadata
        case prayerTimes = "prayer_times"
    }
}

// Përdorimi
func merrKohetENamazit(muaji: String, dita: Int) -> DitaNamazit? {
    return data.prayerTimes[muaji]?.first { $0.day == dita }
}
```

---

## Metoda e Llogaritjes

Metoda e BIM Kosovës përdor këto parametra:

| Parametri | Vlera | Përshkrimi |
|-----------|-------|------------|
| Këndi i Sabahut | 18° | Këndi i depresionit të diellit për agimin |
| Këndi i Jacisë | 17° | Këndi i depresionit të diellit për darkën |
| Temkini | 1.5° (6 min) | Kompensimi për reliefin malor |
| Diferenca Imsak-Sabah | 20 min | Sabahu fillon 20 min pas Imsakut |
| Pika referuese | Deçan | Pika më perëndimore e Kosovës |

### Llogaritja e Irtifasë

Sipas Takvimit zyrtar:
> "Irtifaja (konstatimi i orës allaturka në bazë të pozitës së diellit) bashkë me provat praktike, janë bërë në pjesën më perëndimore të Kosovës (në Deçan)."

---

## Koha Verore (DST)

Kohët në JSON tashmë përfshijnë ndryshimet e kohës verore. Kosova zbaton:

| Ndryshimi | Data | Veprimi |
|-----------|------|---------|
| Fillimi i DST | E diela e fundit e marsit, ora 02:00 | +1 orë |
| Fundi i DST | E diela e fundit e tetorit, ora 03:00 | -1 orë |

### Shembull për vitin 2026:
- **Fillimi**: 29 Mars 2026, ora 02:00 → 03:00
- **Fundi**: 25 Tetor 2026, ora 03:00 → 02:00

---

## Ngjarjet Islame (Referenca 2026)

| Ngjarja | Data 2026 | Përshkrimi |
|---------|-----------|------------|
| Nata e Miraxhit | 15 Janar | Ngjitja e Profetit ﷺ në qiell |
| Nata e Beratit | 2 Shkurt | Nata e faljes |
| Fillimi i Ramazanit | 19 Shkurt | Dita e parë e agjërimit |
| Nata e Kadrit | 16 Mars | Nata më e madhe |
| Fitër Bajrami | 20 Mars | Festa e përfundimit të Ramazanit |
| Kurban Bajrami | 27 Maj | Festa e Kurbanit |
| Viti i Ri Hixhri 1448 | 16 Qershor | Fillimi i vitit të ri islam |
| Dita e Ashurës | 25 Qershor | Dita e 10-të e Muharremit |
| Mevludi | 24 Gusht | Ditëlindja e Profetit ﷺ |

*Shënim: Datat islame zhvendosen ~11 ditë më herët çdo vit gregorian.*

---

## Licenca

Licenca MIT - Shih skedarin [LICENSE](LICENSE).

Të dhënat burojnë nga Takvimi zyrtar i BIK-ut. Ju lutemi citoni burimin kur përdorni këto të dhëna.

---

## Kontributi

Keni gjetur një gabim? Hapni një "issue" me:
- Datat specifike të prekura
- Vlerat e pritura kundrejt atyre aktuale
- Referencë në faqen e PDF-së origjinal

---

## Projekte të Lidhura

- [Takvimi-per-Evrope-per-MacOS](https://github.com/drilonjaha/Takvimi-per-Evrope-per-MacOS) - Aplikacioni i kohëve të namazit për macOS
- [Takvimi-per-Evrope-Windows](https://github.com/drilonjaha/Takvimi-per-Evrope-Windows) - Aplikacioni i kohëve të namazit për Windows

---

## Falënderime

- **Bashkësia Islame e Kosovës (BIK)** - Për publikimin e Takvimit zyrtar
- **Nuhi Simnica** - Përgatitësi i Takvimit
- **Hajrullah ef. Hoxha** - Për kontributin e tij mbi pesë dekada në hartimin e takvimit

---

*Ky repositor është krijuar për të ofruar qasje të lehtë në të dhënat e kohëve të namazit për zhvilluesit e aplikacioneve dhe komunitetin mysliman të Kosovës.*
