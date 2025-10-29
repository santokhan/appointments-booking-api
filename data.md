# 📘 Appointment Booking Calendar Data Structure

This structure represents *daily appointment bookings* for a calendar system. Each entry contains a *date* and a list of *bookings* scheduled on that date.

---

## 🔹 Root Level

* *Array of Objects* → Each object represents one *day*.

### Day Object

| Key      | Type   | Description                                                      |
| -------- | ------ | ---------------------------------------------------------------- |
| id       | number | Unique identifier for the day entry (can be DB ID or generated). |
| date     | string | Formatted date string (e.g., "Thu, September 11, 2025").         |
| bookings | array  | List of booking objects scheduled for that day.                  |

---

## 🔹 Booking Object

Each booking represents a scheduled appointment, staff event, or time block.

| Key       | Type   | Description                                                             |
| --------- | ------ | ----------------------------------------------------------------------- |
| id        | number | Unique identifier for the booking.                                      |
| title     | string | Short description of the booking (e.g., "Weekly Review").               |
| type      | string | Defines the booking category:                                           |
|           |        | - "staff" → Internal/team meeting                                       |
|           |        | - "customer" → Customer-facing appointment                              |
|           |        | - "block" → Reserved time (not bookable)                                |
| startTime | object | Start time of the booking (24h format with hour and minute as strings). |
| endTime   | object | End time of the booking (24h format with hour and minute as strings).   |

---

## 🔹 Time Object

Both startTime and endTime follow this format:

| Key    | Type   | Description                            |
| ------ | ------ | -------------------------------------- |
| hour   | string | Hour in 24-hour format (e.g., "09").   |
| minute | string | Minute in 2-digit format (e.g., "45"). |

---

## 🔹 Example

```json
[
  {
    "id": 1,
    "date": "Thu, September 11, 2025",
    "bookings": [
      {
        "id": 4,
        "title": "Weekly Review",
        "type": "staff",
        "startTime": { "hour": "09", "minute": "45" },
        "endTime": { "hour": "11", "minute": "15" }
      },
      {
        "id": 5,
        "title": "Sprint Planning",
        "type": "block",
        "startTime": { "hour": "14", "minute": "00" },
        "endTime": { "hour": "15", "minute": "30" }
      },
      {
        "id": 3,
        "title": "Team Lunch",
        "type": "customer",
        "startTime": { "hour": "17", "minute": "00" },
        "endTime": { "hour": "17", "minute": "30" }
      }
    ]
  }
]
```
