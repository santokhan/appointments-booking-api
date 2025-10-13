# **Appointment Booking API Documentation (Team, Client, and Service Info)**

## **Overview**

This API allows managing appointment bookings, including:

* Fetching appointment details (GET)
* Creating new bookings (POST) for staff or blocking time slots
* Canceling or deleting bookings

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

**Response Example:** (same as before)

---

## **2. POST Endpoints for Staff**

### **2.1 Create Staff Booking**

**Endpoint:**

```
POST /api/appointments/booking
```

**Description:**
Create a booking with client and service info.

**Request Body Example:** (same as before)

**Response Example:** (same as before)

---

### **2.2 Create Blocked Time Slot**

**Endpoint:**

```
POST /api/appointments/block
```

**Description:**
Create a blocked time slot for a team member.

**Request Body Example:** (same as before)

**Response Example:** (same as before)

---

## **3. DELETE / Cancel Endpoints**

### **3.1 Cancel Appointment**

**Endpoint:**

```
DELETE /api/appointments/:bookingId
```

**Description:**
Cancel an existing appointment by booking ID. Optionally, a reason can be provided.

**Request Body Example (optional):**

```json
{
  "reason": "Client requested cancellation"
}
```

**Response Example:**

```json
{
  "success": true,
  "message": "Appointment canceled successfully",
  "bookingId": "102"
}
```

---

### **3.2 Delete Blocked Slot**

**Endpoint:**

```
DELETE /api/appointments/block/:blockId
```

**Description:**
Delete an existing blocked time slot by its ID.

**Response Example:**

```json
{
  "success": true,
  "message": "Blocked slot deleted successfully",
  "blockId": "205"
}
```

---

## **4. Developer Notes**

### **4.1 Time Format**

* All `startTime` and `endTime` fields use **camelCase objects**:

  ```json
  { "hour": 10, "minute": 30 }
  ```

### **4.2 Booking Types**

| Type       | Description                                            |
| ---------- | ------------------------------------------------------ |
| `staff`    | Regular staff appointment with client and service info |
| `customer` | Regular customer booking                               |
| `blocked`  | Blocked time slot with reason                          |

### **4.3 Filtering in GET**

* Use `teamMembers` (comma-separated IDs), `days`, and `startDate`.

### **4.4 Conflict Validation**

* Ensure no overlapping bookings for the same team member.
* Blocked slots take precedence over customer/staff bookings.

### **4.5 Models Reference**

* Use **Appointment**, **BlockingTimeSlot**, and **Vacant** models for field mapping.

**Note:* Do not forgot to build **Edit** API endpoint as well. ⭐
