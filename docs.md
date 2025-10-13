# **Appointment Booking API Documentation (Team, Client, and Service Info)**

## **Overview**

This API allows managing appointment bookings, including:

* Fetching appointment details (GET)
* Creating new bookings (POST) for staff or blocking time slots

**Main Models:**

1. **Appointment** – standard booking model
2. **BlockingTimeSlot** – to block specific periods
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
| `teamMembers` | string | No       | Comma-separated list of team member IDs to filter by                        |
| `days`        | int    | No       | Number of days to fetch from `startDate`. Default: 7                        |
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
          "startTime": { "hour": 12, "minute": 0 },
          "endTime": { "hour": 12, "minute": 30 }
        }
      ]
    }
  ]
}
```

---

## **2. POST Endpoints for Staff**

### **2.1 Create Staff Booking**

**Endpoint:**

```
POST /api/appointments/booking
```

**Description:**
Create a booking with client and service info.

**Request Body Example:**

```json
{
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

**Response Example:**

```json
{
  "success": true,
  "message": "Booking created successfully",
  "bookingId": "102"
}
```

---

### **2.2 Create Blocked Time Slot**

**Endpoint:**

```
POST /api/appointments/block
```

**Description:**
Create a blocked time slot for a team member.

**Request Body Example:**

```json
{
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
  "message": "Blocked slot created successfully",
  "blockId": "205"
}
```

---

## **3. Developer Notes**

### **3.1 Time Format**

* All `startTime` and `endTime` fields use **camelCase objects**:

  ```json
  { "hour": 10, "minute": 30 }
  ```

### **3.2 Booking Types**

| Type       | Description                                            |
| ---------- | ------------------------------------------------------ |
| `staff`    | Regular staff appointment with client and service info |
| `customer` | Regular customer booking                               |
| `blocked`  | Blocked time slot with reason                          |

### **3.3 Filtering in GET**

* Use `teamMembers` (comma-separated IDs), `days`, and `startDate`.

### **3.4 Conflict Validation**

* Ensure no overlapping bookings for the same team member.
* Blocked slots take precedence over customer/staff bookings.

### **3.5 Models Reference**

* Use **Appointment**, **BlockingTimeSlot**, and **Vacant** models for field mapping.

---

✅ **Changes made:**

1. Removed duplicated notes about time format and conflict validation.
2. Standardized `hour` and `minute` representation to integers in all examples.
3. Cleaned up headings and numbering for consistency.
4. Clarified booking types in a table format.
5. Minor wording adjustments for clarity.
