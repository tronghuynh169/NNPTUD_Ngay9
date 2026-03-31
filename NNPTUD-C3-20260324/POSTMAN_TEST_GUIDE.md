# Hướng dẫn nhanh - Testing Messages API trên Postman

## 🔐 Không quên thêm Token!
Tất cả requests cần header:
```
Authorization: Bearer <your_jwt_token>
```

---

## 📝 Test Case 1: GET / - Lấy tin nhắn cuối cùng

```
GET http://localhost:3000/api/v1/messages/
Authorization: Bearer <token>
```

**Kết quả mong đợi:**
- Status: 200 OK
- Response: Array of conversations with last message

---

## 📨 Test Case 2: POST / - Gửi tin nhắn TEXT

```
POST http://localhost:3000/api/v1/messages/
Authorization: Bearer <token>
Content-Type: application/json

Body (raw):
{
  "to": "6709c1c2b5a6e1234567890a",
  "type": "text",
  "text": "Xin chào! Bạn khỏe không?"
}
```

**Kết quả mong đợi:**
- Status: 201 Created
- Response: Message object với from, to, messageContent

---

## 📁 Test Case 3: POST / - Gửi tin nhắn FILE

```
POST http://localhost:3000/api/v1/messages/
Authorization: Bearer <token>
Content-Type: application/json

Body (raw):
{
  "to": "6709c1c2b5a6e1234567890a",
  "type": "file",
  "text": "uploads/document.pdf"
}
```

**Kết quả mong đợi:**
- Status: 201 Created
- Response: Message object với type="file"

---

## 💬 Test Case 4: GET /:userID - Lấy tất cả tin nhắn

```
GET http://localhost:3000/api/v1/messages/6709c1c2b5a6e1234567890b
Authorization: Bearer <token>
```

*Replace ID với user ID thực tế*

**Kết quả mong đợi:**
- Status: 200 OK
- Response: Array all messages between 2 users (both directions)

---

## ❌ Test Case 5: Error - Missing Field

```
POST http://localhost:3000/api/v1/messages/
Authorization: Bearer <token>
Content-Type: application/json

Body (raw - missing "to"):
{
  "type": "text",
  "text": "Hello"
}
```

**Kết quả mong đợi:**
- Status: 400 Bad Request
- Error: "Missing required fields: to, type, text"

---

## ❌ Test Case 6: Error - Invalid Type

```
POST http://localhost:3000/api/v1/messages/
Authorization: Bearer <token>
Content-Type: application/json

Body (raw - invalid type):
{
  "to": "6709c1c2b5a6e1234567890a",
  "type": "invalid",
  "text": "Hello"
}
```

**Kết quả mong đợi:**
- Status: 400 Bad Request
- Error: "Type must be 'file' or 'text'"

---

## 📊 Sample Response - POST (Create Message)

```json
{
  "_id": "67129a4f2b8c1a0012345abc",
  "from": {
    "_id": "6709c1c2b5a6e1234567890a",
    "username": "user1",
    "email": "user1@example.com",
    "avatarUrl": "https://i.sstatic.net/l60Hf.png",
    "fullName": "Người Dùng 1"
  },
  "to": {
    "_id": "6709c1c2b5a6e1234567890b",
    "username": "user2",
    "email": "user2@example.com",
    "avatarUrl": "https://i.sstatic.net/l60Hf.png",
    "fullName": "Người Dùng 2"
  },
  "messageContent": {
    "type": "text",
    "text": "Xin chào bạn"
  },
  "isDeleted": false,
  "createdAt": "2026-03-24T10:35:42.123Z",
  "updatedAt": "2026-03-24T10:35:42.123Z"
}
```

---

## 📊 Sample Response - GET / (Last Messages)

```json
[
  {
    "user": {
      "_id": "6709c1c2b5a6e1234567890b",
      "username": "user2",
      "email": "user2@example.com",
      "avatarUrl": "https://i.sstatic.net/l60Hf.png",
      "fullName": "Người Dùng 2"
    },
    "lastMessage": {
      "_id": "67129a4f2b8c1a0012345abc",
      "from": {...},
      "to": {...},
      "messageContent": {
        "type": "text",
        "text": "Xin chào bạn"
      },
      "createdAt": "2026-03-24T10:35:42.123Z",
      "updatedAt": "2026-03-24T10:35:42.123Z"
    }
  }
]
```

---

## 📊 Sample Response - GET /:userID (All Messages)

```json
[
  {
    "_id": "67129a4f2b8c1a0012345aaa",
    "from": {
      "_id": "6709c1c2b5a6e1234567890a",
      "username": "user1",
      "email": "user1@example.com",
      "avatarUrl": "https://i.sstatic.net/l60Hf.png",
      "fullName": "Người Dùng 1"
    },
    "to": {
      "_id": "6709c1c2b5a6e1234567890b",
      "username": "user2",
      "email": "user2@example.com",
      "avatarUrl": "https://i.sstatic.net/l60Hf.png",
      "fullName": "Người Dùng 2"
    },
    "messageContent": {
      "type": "text",
      "text": "Hi there"
    },
    "createdAt": "2026-03-24T10:30:00.000Z",
    "updatedAt": "2026-03-24T10:30:00.000Z"
  },
  {
    "_id": "67129a4f2b8c1a0012345bbb",
    "from": {
      "_id": "6709c1c2b5a6e1234567890b",
      "username": "user2",
      "email": "user2@example.com",
      "avatarUrl": "https://i.sstatic.net/l60Hf.png",
      "fullName": "Người Dùng 2"
    },
    "to": {
      "_id": "6709c1c2b5a6e1234567890a",
      "username": "user1",
      "email": "user1@example.com",
      "avatarUrl": "https://i.sstatic.net/l60Hf.png",
      "fullName": "Người Dùng 1"
    },
    "messageContent": {
      "type": "text",
      "text": "Hello! I'm fine, thanks"
    },
    "createdAt": "2026-03-24T10:32:00.000Z",
    "updatedAt": "2026-03-24T10:32:00.000Z"
  }
]
```

---

## Lưu ý quan trọng

1. **Thay ID thực tế**: Copy user ID từ database hoặc response login
2. **Thay Token**: Logout rồi login lại để lấy token mới nếu hết hạn
3. **Content-Type**: Luôn là `application/json`
4. **Method**: GET vs POST - đừng lẫn
5. **Sorting**: 
   - GET / returns by createdAt descending
   - GET /:userID returns by createdAt ascending
