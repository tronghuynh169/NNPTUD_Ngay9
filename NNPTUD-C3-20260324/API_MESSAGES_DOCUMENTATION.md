# API Documentation - Messages Endpoints

## Base URL
```
http://localhost:3000/api/v1/messages
```

## Authentication
Tất cả các endpoint cần token xác thực:
- **Cookie**: `TOKEN_NNPTUD_C3=<token>`
- **Hoặc Header**: `Authorization: Bearer <token>`

---

## Endpoint 1: GET / - Lấy tin nhắn cuối cùng từ mỗi cuộc hội thoại

### Request
- **Method**: GET
- **URL**: `http://localhost:3000/api/v1/messages/`
- **Headers**:
  ```
  Authorization: Bearer <your_token>
  Content-Type: application/json
  ```

### Response (200 OK)
```json
[
  {
    "user": {
      "_id": "user_id_1",
      "username": "john_doe",
      "email": "john@example.com",
      "avatarUrl": "https://...",
      "fullName": "John Doe"
    },
    "lastMessage": {
      "_id": "message_id_1",
      "from": {...},
      "to": {...},
      "messageContent": {
        "type": "text",
        "text": "Xin chào"
      },
      "createdAt": "2026-03-24T10:30:00.000Z",
      "updatedAt": "2026-03-24T10:30:00.000Z"
    }
  }
]
```

---

## Endpoint 2: POST / - Gửi một tin nhắn mới

### Request
- **Method**: POST
- **URL**: `http://localhost:3000/api/v1/messages/`
- **Headers**:
  ```
  Authorization: Bearer <your_token>
  Content-Type: application/json
  ```

### Request Body

**Ví dụ 1: Gửi tin nhắn text**
```json
{
  "to": "user_id_2",
  "type": "text",
  "text": "Xin chào bạn"
}
```

**Ví dụ 2: Gửi tin nhắn file (chứa đường dẫn file)**
```json
{
  "to": "user_id_2",
  "type": "file",
  "text": "uploads/document.pdf"
}
```

### Response (201 Created)
```json
{
  "_id": "message_id_1",
  "from": {
    "_id": "current_user_id",
    "username": "current_user",
    "email": "current@example.com",
    "avatarUrl": "https://...",
    "fullName": "Current User"
  },
  "to": {
    "_id": "user_id_2",
    "username": "john_doe",
    "email": "john@example.com",
    "avatarUrl": "https://...",
    "fullName": "John Doe"
  },
  "messageContent": {
    "type": "text",
    "text": "Xin chào bạn"
  },
  "createdAt": "2026-03-24T10:35:00.000Z",
  "updatedAt": "2026-03-24T10:35:00.000Z"
}
```

### Error Cases
- **400**: Missing required fields (to, type, text)
- **400**: Invalid type (must be 'file' or 'text')

---

## Endpoint 3: GET /:userID - Lấy toàn bộ tin nhắn giữa user hiện tại và userID

### Request
- **Method**: GET
- **URL**: `http://localhost:3000/api/v1/messages/user_id_2`
- **Headers**:
  ```
  Authorization: Bearer <your_token>
  Content-Type: application/json
  ```

### Response (200 OK)
```json
[
  {
    "_id": "message_id_1",
    "from": {
      "_id": "current_user_id",
      "username": "current_user",
      "email": "current@example.com",
      "avatarUrl": "https://...",
      "fullName": "Current User"
    },
    "to": {
      "_id": "user_id_2",
      "username": "john_doe",
      "email": "john@example.com",
      "avatarUrl": "https://...",
      "fullName": "John Doe"
    },
    "messageContent": {
      "type": "text",
      "text": "Xin chào"
    },
    "createdAt": "2026-03-24T10:30:00.000Z",
    "updatedAt": "2026-03-24T10:30:00.000Z"
  },
  {
    "_id": "message_id_2",
    "from": {
      "_id": "user_id_2",
      "username": "john_doe",
      "email": "john@example.com",
      "avatarUrl": "https://...",
      "fullName": "John Doe"
    },
    "to": {
      "_id": "current_user_id",
      "username": "current_user",
      "email": "current@example.com",
      "avatarUrl": "https://...",
      "fullName": "Current User"
    },
    "messageContent": {
      "type": "text",
      "text": "Xin chào bạn. Bạn khỏe không?"
    },
    "createdAt": "2026-03-24T10:32:00.000Z",
    "updatedAt": "2026-03-24T10:32:00.000Z"
  }
]
```

---

## Thông tin về Message Schema

### Message Structure
```javascript
{
  from: ObjectId (ref: user),           // User gửi tin nhắn
  to: ObjectId (ref: user),             // User nhận tin nhắn
  messageContent: {
    type: String ("file" | "text"),     // Loại nội dung
    text: String                        // Nội dung (text hoặc đường dẫn file)
  },
  isDeleted: Boolean (default: false),  // Đã xóa hay không
  createdAt: Date (auto),               // Thời gian tạo
  updatedAt: Date (auto)                // Thời gian cập nhật
}
```

---

## Hướng dẫn Testing trên Postman

### 1. Thiết lập Environment
1. Tạo Environment mới: `NNPTUD-C3`
2. Thêm biến:
   - `baseUrl`: `http://localhost:3000`
   - `token`: `<your_jwt_token>`

### 2. Test Endpoint 1 (GET /)
1. **Method**: GET
2. **URL**: `{{baseUrl}}/api/v1/messages/`
3. **Headers**:
   - `Authorization`: `Bearer {{token}}`
4. **Send** và kiểm tra kết quả

### 3. Test Endpoint 2 (POST /)
1. **Method**: POST
2. **URL**: `{{baseUrl}}/api/v1/messages/`
3. **Headers**:
   - `Authorization`: `Bearer {{token}}`
   - `Content-Type`: `application/json`
4. **Body** (raw JSON):
   ```json
   {
     "to": "user_id_2",
     "type": "text",
     "text": "Xin chào bạn"
   }
   ```
5. **Send** và kiểm tra kết quả (201)

### 4. Test Endpoint 3 (GET /:userID)
1. **Method**: GET
2. **URL**: `{{baseUrl}}/api/v1/messages/user_id_2`
3. **Headers**:
   - `Authorization`: `Bearer {{token}}`
4. **Send** và kiểm tra kết quả
