# 🛠️ React Product Management App

A simple React product management application using **form inputs**, **CRUD operations**, **Axios for API calls**, and **React Router** for navigation. Data is persisted via a local JSON server (`http://localhost:3000/users`).

---

## 📁 Project Structure

```
src/
├── App.js
├── pages/
│   ├── Home.jsx
│   ├── Form.jsx
│   └── Datatable.jsx
```

---

## 🚦 Routing Overview

| Route        | Component   | Purpose                     |
|--------------|-------------|-----------------------------|
| `/`          | `Home`      | Landing page                |
| `/form`      | `Form`      | Add/Edit product form       |
| `/datatable` | `Datatable` | Product list with actions   |

---

## 🧠 App.js — Main Controller

**Purpose**: Handles global state, API logic with Axios, validation, and routing.

### 📦 State:

- `user`: Current product/user form data.
- `list`: List of all users/products.
- `error`: Validation messages.
- `editIdx`: Used for determining edit mode.

### 🔁 Functions:

```js
useEffect(() => {
  handleFetch();
}, []);
```

```js
const handleChange = (e) => {
  const { name, value } = e.target;
  setUser({ ...user, [name]: value });
};
```

```js
const handleFetch = async () => {
  const res = await axios.get(url);
  setList(res.data);
};
```

```js
const handleSubmit = async (e) => {
  e.preventDefault();
  if (!validation()) return;

  if (editIdx === "") {
    await axios.post(url, { ...user, id: String(Date.now()) });
  } else {
    await axios.put(`${url}/${editIdx}`, user);
    setEditIdx("");
  }

  handleFetch();
  navi("/datatable");
  setUser({});
};
```

```js
const handleEdit = (id) => {
  const item = list.find((el) => el.id === id);
  setUser(item);
  setEditIdx(id);
  navi("/form");
};
```

```js
const handleDelete = async (id) => {
  await axios.delete(`${url}/${id}`);
  handleFetch();
};
```

```js
const validation = () => {
  const error = {};
  if (!user.fullName) error.fullName = "Full Name is required";
  if (!user.email) error.email = "Email is required";
  if (!user.password) error.password = "Password is required";
  if (!user.age) error.age = "Age is required";
  setError(error);
  return Object.keys(error).length === 0;
};
```

---

## 🏠 Home.jsx

**Purpose**: Simple landing page.

### Features:
- Welcome text.
- Buttons to navigate to:
  - Add Product (`/form`)
  - View Products (`/datatable`)

---

## 📝 Form.jsx

**Purpose**: To add or edit a product.

### Props:
- `handleChange`
- `handleSubmit`
- `user`
- `error`

### Features:
- Input fields: Full Name, Email, Password, Age.
- Inline validation errors.

---

## 📊 Datatable.jsx

**Purpose**: Lists all users/products.

### Props:
- `list`
- `handleEdit`
- `handleDelete`

### Features:
- Table view: name, email, etc.
- Action buttons: Edit and Delete.
- Shows message if the list is empty.

---

## 🔗 JSON Server Setup

To run the mock API, install and start **JSON Server**:

```bash
npm install -g json-server
json-server --watch db.json --port 3000
```

Example `db.json`:

```json
{
  "users": []
}
```

---
