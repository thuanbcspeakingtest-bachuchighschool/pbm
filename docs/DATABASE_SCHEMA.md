# 🗄️ Database Schema

## 1. Collections/Tables

### 1.1 Users
```json
{
  "_id": "ObjectId",
  "email": "string (unique, indexed)",
  "password": "string (hashed)",
  "fullName": "string",
  "role": "enum: ADMIN | BGH | GV_MANAGER | GV_STAFF",
  "department": "string",
  "assignedRoom": "ObjectId (null if not manager)",
  "status": "enum: ACTIVE | INACTIVE",
  "createdAt": "timestamp",
  "updatedAt": "timestamp",
  "lastLogin": "timestamp"
}

// Indexes
db.users.createIndex({ email: 1 }, { unique: true })
db.users.createIndex({ role: 1 })
db.users.createIndex({ assignedRoom: 1 })
```

### 1.2 Rooms
```json
{
  "_id": "ObjectId",
  "code": "string (unique: PA-EN, PA-BIO, ...)",
  "name": "string (Phòng BM Tiếng Anh, ...)",
  "managerId": "ObjectId (User)",
  "capacity": "number",
  "location": "string",
  "status": "enum: ACTIVE | MAINTENANCE | INACTIVE",
  "description": "string",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}

// Indexes
db.rooms.createIndex({ code: 1 }, { unique: true })
db.rooms.createIndex({ managerId: 1 })
db.rooms.createIndex({ status: 1 })
```

### 1.3 Equipment
```json
{
  "_id": "ObjectId",
  "roomId": "ObjectId (Room)",
  "name": "string (Máy chiếu, Bảng tương tác, ...)",
  "code": "string (unique)",
  "category": "enum: AUDIO | VISUAL | COMPUTER | FURNITURE | OTHER",
  "quantity": "number",
  "status": "enum: GOOD | FAULTY | MAINTENANCE | MISSING",
  "purchaseDate": "date",
  "warranty": "date",
  "notes": "string",
  "lastUpdated": "timestamp",
  "updatedBy": "ObjectId (User)"
}

// Indexes
db.equipment.createIndex({ roomId: 1 })
db.equipment.createIndex({ code: 1 }, { unique: true })
db.equipment.createIndex({ status: 1 })
```

### 1.4 BookingSchedules
```json
{
  "_id": "ObjectId",
  "roomId": "ObjectId (Room)",
  "userId": "ObjectId (User - GV Staff)",
  "date": "date",
  "period": "number (1-10)",
  "class": "string (10A1, 11B2, ...)",
  "subject": "string",
  "statusBefore": "enum: CLEAN | DIRTY | NEEDS_EQUIPMENT | UNKNOWN",
  "statusAfter": "enum: CLEAN | DIRTY | EQUIPMENT_ISSUE | NOT_CHECKED",
  "notes": "string",
  "approvalStatus": "enum: PENDING | APPROVED | REJECTED",
  "approvedBy": "ObjectId (User - GV Manager)",
  "rejectionReason": "string (if rejected)",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}

// Indexes
db.bookingSchedules.createIndex({ roomId: 1, date: 1 })
db.bookingSchedules.createIndex({ userId: 1 })
db.bookingSchedules.createIndex({ approvalStatus: 1 })
db.bookingSchedules.createIndex({ date: 1 })
```

### 1.5 RoomManagers
```json
{
  "_id": "ObjectId",
  "roomId": "ObjectId (Room)",
  "teacherId": "ObjectId (User - GV Staff allowed)",
  "permission": "enum: VIEW | BOOK | MANAGE",
  "assignedAt": "timestamp",
  "assignedBy": "ObjectId (User - GV Manager)"
}

// Indexes
db.roomManagers.createIndex({ roomId: 1 })
db.roomManagers.createIndex({ teacherId: 1 })
db.roomManagers.createIndex({ roomId: 1, teacherId: 1 }, { unique: true })
```

### 1.6 Statistics
```json
{
  "_id": "ObjectId",
  "roomId": "ObjectId (Room)",
  "month": "number (1-12)",
  "year": "number",
  "totalUsage": "number",
  "totalTeachers": "number",
  "teacherUsage": [
    {
      "teacherId": "ObjectId",
      "teacherName": "string",
      "usageCount": "number",
      "lastUsed": "date"
    }
  ],
  "equipmentIssues": "number",
  "averageRoomCondition": "number (1-5)",
  "peakPeriods": [
    {
      "period": "number",
      "count": "number"
    }
  ],
  "createdAt": "timestamp"
}

// Indexes
db.statistics.createIndex({ roomId: 1, year: 1, month: 1 }, { unique: true })
```

### 1.7 Audit Logs
```json
{
  "_id": "ObjectId",
  "userId": "ObjectId (User)",
  "action": "string (CREATE, UPDATE, DELETE, APPROVE, REJECT)",
  "resource": "string (Room, Equipment, Booking, User)",
  "resourceId": "ObjectId",
  "changes": {
    "field": "value"
  },
  "ipAddress": "string",
  "userAgent": "string",
  "timestamp": "timestamp"
}

// Indexes
db.auditLogs.createIndex({ userId: 1 })
db.auditLogs.createIndex({ resourceId: 1 })
db.auditLogs.createIndex({ timestamp: 1 })
```

## 2. Quan Hệ Dữ Liệu

```
Users (1) ──────→ (many) Rooms (as manager)
Users (many) ──→ (many) RoomManagers
Rooms (1) ──────→ (many) Equipment
Rooms (1) ──────→ (many) BookingSchedules
Users (1) ──────→ (many) BookingSchedules (as booker)
Users (1) ──────→ (many) BookingSchedules (as approver)
Rooms (1) ──────→ (many) Statistics
```

## 3. Ví Dụ Dữ Liệu

### Admin User
```json
{
  "_id": ObjectId("..."),
  "email": "admin@school.edu.vn",
  "password": "$2b$10$...",
  "fullName": "Quản trị viên hệ thống",
  "role": "ADMIN",
  "status": "ACTIVE",
  "createdAt": ISODate("2026-01-01T00:00:00Z")
}
```

### Room
```json
{
  "_id": ObjectId("..."),
  "code": "PA-EN",
  "name": "Phòng BM Tiếng Anh",
  "managerId": ObjectId("..."),
  "capacity": 40,
  "location": "Tầng 2, Nhà A",
  "status": "ACTIVE",
  "createdAt": ISODate("2026-01-01T00:00:00Z")
}
```

### Equipment
```json
{
  "_id": ObjectId("..."),
  "roomId": ObjectId("..."),
  "name": "Máy chiếu",
  "code": "PROJ-001",
  "category": "VISUAL",
  "quantity": 1,
  "status": "GOOD",
  "purchaseDate": ISODate("2024-06-01T00:00:00Z"),
  "warranty": ISODate("2026-06-01T00:00:00Z"),
  "notes": "Bảo hành 2 năm",
  "lastUpdated": ISODate("2026-04-10T00:00:00Z")
}
```

### Booking
```json
{
  "_id": ObjectId("..."),
  "roomId": ObjectId("..."),
  "userId": ObjectId("..."),
  "date": ISODate("2026-04-15T00:00:00Z"),
  "period": 3,
  "class": "10A1",
  "subject": "English",
  "statusBefore": "CLEAN",
  "statusAfter": "CLEAN",
  "approvalStatus": "APPROVED",
  "approvedBy": ObjectId("..."),
  "createdAt": ISODate("2026-04-10T08:00:00Z")
}
```

## 4. Thao Tác Cơ Bản

### Create User
```javascript
db.users.insertOne({
  email: "gv01@school.edu.vn",
  password: bcryptHash("password123"),
  fullName: "Thầy Nguyễn Văn A",
  role: "GV_MANAGER",
  assignedRoom: roomId,
  status: "ACTIVE"
})
```

### Update Equipment Status
```javascript
db.equipment.updateOne(
  { _id: ObjectId("...") },
  {
    $set: {
      status: "FAULTY",
      notes: "Đèn LED không sáng",
      lastUpdated: new Date()
    }
  }
)
```

### Get Bookings for Date Range
```javascript
db.bookingSchedules.find({
  roomId: ObjectId("..."),
  date: {
    $gte: ISODate("2026-04-01"),
    $lte: ISODate("2026-04-30")
  },
  approvalStatus: "APPROVED"
})
```

### Get Statistics
```javascript
db.statistics.findOne({
  roomId: ObjectId("..."),
  year: 2026,
  month: 4
})
```

## 5. Performance Optimization

- Index trên các trường thường được query (email, role, status, date)
- Soft delete (thêm field `deletedAt`) thay vì xóa trực tiếp
- Caching Redis cho statistics & reports
- Pagination cho danh sách (limit 10-20 items)
- Aggregation pipeline cho các báo cáo phức tạp

---
**Last Updated**: 2026-04-10