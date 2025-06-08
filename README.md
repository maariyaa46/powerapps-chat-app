# 💬 Power Apps – WhatsApp-Like Chat Application (Canvas App)

This repository contains a WhatsApp-style **Chat App built using Power Apps Canvas App** with **Dataverse** as the backend. The app replicates core messaging functionalities like user registration, chat initiation, real-time messaging, and profile management.

---

## 🧩 Features

- 🔐 **User Registration and Login**
- 📇 **Contacts List** (excluding the current user)
- 💬 **Chat Interface** with WhatsApp-style UX
- 📨 **Start New Conversations**
- 📜 **Chat History View**
- 📝 **Edit Profile**
- 🧠 **Smart message filtering** using composite keys

---

## 🔗 Data Source: Microsoft Dataverse

The app uses **three Dataverse tables**:

### 1. `ChatUser`
Stores all user details.

| Column Name       | Type     | Description                       |
|-------------------|----------|-----------------------------------|
| User Name         | Text     | Full name of the user             |
| Email             | Email    | User's email                      |
| Phone Number      | Text     | Unique phone number (used as ID) |
| Profile Picture   | Image    | User's display photo              |

---

### 2. `Conversation`

Defines chats between two users.

| Column Name         | Type   | Description                                 |
|---------------------|--------|---------------------------------------------|
| Participant 1       | Lookup | Lookup to `ChatUser` (logged-in user)       |
| Participant 2       | Lookup | Lookup to `ChatUser` (selected contact)     |
| Conversation Key    | Text   | Concatenation of both phone numbers, used to uniquely identify conversations |

> Example: If A = 111 and B = 222, key = `111_222` (order-independent)

---

### 3. `Message`

Stores chat messages linked to a conversation.

| Column Name         | Type     | Description                                   |
|---------------------|----------|-----------------------------------------------|
| Message Content     | Text     | The actual message text                       |
| Timestamp           | DateTime | Sent time                                     |
| Conversation        | Lookup   | Related record in `Conversation` table        |
| Conversation Key    | Text     | Same format as in `Conversation` table (not a lookup) |

---

## 🧭 How the App Works

### 1. 🔐 **Registration & Login**
- User logs in using their phone number.
- If the number is not found in `ChatUser`, they are redirected to a **registration form**.
- Once registered, user is logged in and redirected to the main **Chats screen**.

---

### 2. 💬 **Chats Screen**
- Shows all ongoing conversations of the logged-in user.
- Conversations are filtered using the composite **Conversation Key** (e.g., `111_222`).
- Displays last message and timestamp (like WhatsApp).

---

### 3. 📇 **Contacts Screen**
- Lists all users in the system **except the logged-in user**.
- Selecting a contact:
  - Checks if a conversation exists.
  - If not, a new one is created using a generated Conversation Key.
  - Redirects to the **Messaging screen**.

---

### 4. 📨 **Messaging Screen**
- Displays chat messages between the user and selected contact.
- Messages filtered using `Conversation Key` to ensure accurate history.
- New messages are stored with content, timestamp, and related conversation reference.

---

### 5. 📝 **Edit Profile**
- Users can view and update their name, profile picture, and email address.


