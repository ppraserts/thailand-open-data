# Thailand Open Data

ข้อมูลเปิดของประเทศไทยในรูปแบบ JSON พร้อมใช้งาน
Open data for Thailand in JSON format, ready to use.

## Datasets

| Dataset | Path | Description |
|---------|------|-------------|
| วันหยุดราชการ | [`data/thai-public-holidays/`](data/thai-public-holidays/) | Thai public holidays (วันหยุดราชการไทย) |
| วันหยุดธนาคาร | [`data/thai-bank-holidays/`](data/thai-bank-holidays/) | Thai bank holidays (วันหยุดสถาบันการเงิน) |

### Thai Public Holidays (วันหยุดราชการไทย)

ประกาศโดยสำนักนายกรัฐมนตรี / มติคณะรัฐมนตรี

| File | Description |
|------|-------------|
| [`2025.json`](data/thai-public-holidays/2025.json) | ปี พ.ศ. 2568 (2025) — 24 วัน |
| [`2026.json`](data/thai-public-holidays/2026.json) | ปี พ.ศ. 2569 (2026) — 22 วัน |
| [`all.json`](data/thai-public-holidays/all.json) | รวมทุกปี |

### Thai Bank Holidays (วันหยุดสถาบันการเงิน)

ประกาศโดยธนาคารแห่งประเทศไทย (ธปท.)

| File | Description |
|------|-------------|
| [`2025.json`](data/thai-bank-holidays/2025.json) | ปี พ.ศ. 2568 (2025) — 19 วัน |
| [`2026.json`](data/thai-bank-holidays/2026.json) | ปี พ.ศ. 2569 (2026) — 19 วัน |
| [`all.json`](data/thai-bank-holidays/all.json) | รวมทุกปี |

> **หมายเหตุ:** วันหยุดราชการ vs วันหยุดธนาคาร มีความแตกต่างกัน เช่น
> - วันแรงงาน (1 พ.ค.) — ธนาคารหยุด แต่ไม่ใช่วันหยุดราชการ
> - วันเข้าพรรษา, วันพืชมงคล — วันหยุดราชการ แต่ธนาคารไม่หยุด
> - วันหยุดธนาคารไม่รวมวันที่ตรงกับเสาร์-อาทิตย์ (เพราะธนาคารหยุดอยู่แล้ว)

## Usage

### Fetch via URL

ดึงข้อมูลได้โดยตรงผ่าน GitHub Raw URL:

```
https://raw.githubusercontent.com/ppraserts/thailand-open-data/main/data/thai-public-holidays/2026.json
```

หรือผ่าน GitHub Pages (ถ้าเปิดใช้งาน):

```
https://ppraserts.github.io/thailand-open-data/data/thai-public-holidays/2026.json
```

### JavaScript / TypeScript

```javascript
const res = await fetch(
  "https://raw.githubusercontent.com/ppraserts/thailand-open-data/main/data/thai-public-holidays/2026.json"
);
const data = await res.json();

console.log(`วันหยุดปี ${data.year} มีทั้งหมด ${data.total_holidays} วัน`);
data.holidays.forEach((h) => {
  console.log(`${h.date} - ${h.name_th} (${h.name_en})`);
});
```

### Python

```python
import requests

url = "https://raw.githubusercontent.com/ppraserts/thailand-open-data/main/data/thai-public-holidays/2026.json"
data = requests.get(url).json()

print(f"วันหยุดปี {data['year']} มีทั้งหมด {data['total_holidays']} วัน")
for h in data["holidays"]:
    print(f"{h['date']} - {h['name_th']} ({h['name_en']})")
```

### cURL

```bash
curl -s https://raw.githubusercontent.com/ppraserts/thailand-open-data/main/data/thai-public-holidays/2026.json | jq '.holidays[] | "\(.date) \(.name_en)"'
```

## JSON Schema

### Holiday Object

| Field | Type | Description |
|-------|------|-------------|
| `date` | `string` | วันที่ (YYYY-MM-DD, ปฏิทิน CE) |
| `name_th` | `string` | ชื่อวันหยุด (ภาษาไทย) |
| `name_en` | `string` | ชื่อวันหยุด (English) |
| `type` | `string` | ประเภทวันหยุด (ดูด้านล่าง) |
| `is_substitution` | `boolean` | เป็นวันหยุดชดเชยหรือไม่ |
| `note` | `string\|null` | หมายเหตุเพิ่มเติม |

### Holiday Types

| Type | Description |
|------|-------------|
| `public_holiday` | วันหยุดราชการตามปกติ |
| `bank_holiday` | วันหยุดสถาบันการเงิน (ประกาศโดย ธปท.) |
| `substitution_holiday` | วันหยุดชดเชย |
| `special_holiday` | วันหยุดพิเศษตามมติคณะรัฐมนตรี / ธปท. |
| `government_holiday` | วันหยุดเฉพาะภาคราชการ (เช่น วันพืชมงคล) |

## GitHub Pages Setup

หากต้องการให้ fetch ผ่าน GitHub Pages (รองรับ CORS ดีกว่า):

1. ไปที่ **Settings** > **Pages** ใน GitHub repo
2. เลือก **Source**: Deploy from a branch
3. เลือก **Branch**: `main` / `/ (root)`
4. กด **Save**

URL จะพร้อมใช้งานที่: `https://ppraserts.github.io/thailand-open-data/`

## Data Sources

- [สำนักงานพัฒนารัฐบาลดิจิทัล (DGA)](https://www.dga.or.th/) — วันหยุดราชการประจำปี
- [ราชกิจจานุเบกษา](https://ratchakitcha.soc.go.th/) — ประกาศวันหยุดราชการ
- [มติคณะรัฐมนตรี](https://www.thaigov.go.th/) — วันหยุดพิเศษเพิ่มเติม
- [ธนาคารแห่งประเทศไทย (ธปท.)](https://www.bot.or.th/th/financial-institutions-holiday.html) — วันหยุดสถาบันการเงิน

## Contributing

ยินดีรับ contribution ครับ! สามารถช่วยเพิ่มข้อมูลได้ เช่น:

- เพิ่มวันหยุดราชการปีอื่นๆ
- เพิ่มชุดข้อมูลใหม่ (จังหวัด, รหัสไปรษณีย์, ฯลฯ)
- แก้ไขข้อมูลที่ผิดพลาด

กรุณา fork repo แล้วส่ง Pull Request มาได้เลยครับ

## License

[MIT License](LICENSE) - ใช้งานได้อิสระทั้งส่วนตัวและเชิงพาณิชย์
