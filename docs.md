# **Appointment Booking API Documentation (with Team, Client, and Service Info)**

## **Overview**

This API allows managing appointment bookings, including:

* Fetching appointment details (GET)
* Creating new bookings (POST) for staff or blocking time slots

We have three main models for reference:

1. **Appointment** – standard booking model
2. **Blocking Time Slot** – to block specific periods
3. **Vacant** – optional reference for available slots

---

## **1. Get Appointment Bookings**

**Endpoint:**

```
GET /api/appointments
```

**Description:**
Fetch all appointment bookings with optional filters: by team members, number of days, and start date.

**Query Parameters:**

| Parameter     | Type   | Required | Description                                                                 |
| ------------- | ------ | -------- | --------------------------------------------------------------------------- |
| `teamMembers` | string | No       | Comma-separated list of team member IDs to filter by.                       |
| `days`        | int    | No       | Number of days to fetch from the `startDate`. Default: 7                    |
| `startDate`   | string | No       | Start date for fetching appointments in `YYYY-MM-DD` format. Default: today |

**Response Example:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Appointments fetched successfully",
    "data": [
        {
            "id": 1,
            "date": "Mon, December 15, 2025",
            "bookings": [
                {
                    "id": "68dccb3623d230265a9931d5",
                    "title": "General Appointment",
                    "type": "customer",
                    "teamMember": {
                        "id": "101",
                        "name": "Alice Johnson",
                        "role": "Therapist"
                    },
                    "client": {
                        "id": "501",
                        "name": "John Doe",
                        "contact": "+1234567890"
                    },
                    "service": {
                        "id": "301",
                        "name": "Full Body Massage",
                        "duration": 30
                    },
                    "startTime": { "hour": "12", "minute": "00" },
                    "endTime": { "hour": "12", "minute": "30" }
                }
            ]
        }
    ]
}
```

---

## **2. Create Appointment Booking**

**Endpoint:**

```
POST /api/appointments
```

**Description:**
Create a new booking. Booking type can be either:

* `staff` – a staff booking
* `blocked` – a blocked time slot

**Request Body:**

### **Staff Booking Example**

```json
{
    "type": "staff",
    "teamMember": {
        "id": "101",
        "name": "Alice Johnson",
        "role": "Therapist"
    },
    "client": {
        "id": "501",
        "name": "John Doe",
        "contact": "+1234567890"
    },
    "service": {
        "id": "301",
        "name": "Full Body Massage",
        "duration": 30
    },
    "startTime": { "hour": 10, "minute": 0 },
    "endTime": { "hour": 11, "minute": 0 },
    "notes": "Optional notes here"
}
```

### **Blocked Time Slot Example**

```json
{
    "type": "blocked",
    "teamMember": {
        "id": "105",
        "name": "Admin",
        "role": "Manager"
    },
    "startTime": { "hour": 12, "minute": 0 },
    "endTime": { "hour": 13, "minute": 0 },
    "reason": "Maintenance / Vacation"
}
```

**Response Example:**

```json
{
    "success": true,
    "message": "Appointment created successfully",
    "appointmentId": "102"
}
```

---

## **3. Notes for Developer**

1. **Time Format:**

   * All startTime and endTime fields use **camelCase objects**: `{ hour: int, minute: int }`.

2. **Booking Types:**

   * `staff` – regular staff appointment with client and service info
   * `customer` – regular customer booking
   * `blocked` – blocked time slot with reason

3. **Filtering in GET:**

   * Use `teamMembers` (comma-separated IDs), `days`, and `startDate`

4. **Conflict Validation:**

   * Ensure no overlapping bookings for the same teamMember
   * Blocked slots take precedence over customer/staff bookings

5. **Models Reference:**

   * Use **Appointment**, **BlockingTimeSlot**, and **Vacant** models for field mapping
