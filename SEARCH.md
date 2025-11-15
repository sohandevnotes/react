# 🔍 React + Node.js Search Functionality Documentation

This document explains how to implement **Search** functionality in a **React frontend** and **Node.js backend** for fetching and filtering data (applications) based on user search input.

---

## 🚀 Backend Search API

### **Endpoint**

```
GET /apps?limit=10&skip=0&sort=size&order=asc&search=appName
````

### **Query Parameters**

| Param   | Description                                              | Example           |
|---------|----------------------------------------------------------|-------------------|
| `limit` | Number of items to fetch per page                        | `limit=10`        |
| `skip`  | Number of items to skip (pagination)                     | `skip=0`          |
| `sort`  | Field to sort by (e.g., `rating`, `size`, etc.)          | `sort=rating`     |
| `order` | Sorting order (`asc` = low → high, `desc` = high → low)  | `order=desc`      |
| `search`| Search query to filter results based on app title        | `search=calculator`|

---

## ✅ Backend Search Code

```javascript
app.get("/apps", async (req, res) => {
  try {
    const {
      limit = 0,
      skip = 0,
      sort = "size",
      order = "desc",
      search = "",
    } = req.query;

    const sortOption = {};
    sortOption[sort || "size"] = order === "asc" ? 1 : -1;

    // Filter query based on search text
    const query = search ? { title: { $regex: search, $options: "i" } } : {};

    const apps = await appsCollection
      .find(query)
      .sort(sortOption)
      .limit(parseInt(limit))
      .skip(parseInt(skip))
      .project({ description: 0, ratings: 0 }) // Exclude certain fields to reduce data load
      .toArray();

    const count = await appsCollection.countDocuments(query); // Total count for pagination

    res.send({ apps, total: count });
  } catch (error) {
    console.log(error);
    res.status(500).json({ error: "Internal Server Error" });
  }
});
````

### ✔ How Backend Search Works

* **Search Field**: The search query (`search`) is used to filter app titles using a **regex** query.
* **Case Insensitive**: The search is **case-insensitive** (`$options: "i"`).
* **Pagination**: The backend handles pagination using `limit` and `skip`.
* **Sorting**: The search results are also sorted based on the `sort` and `order` query parameters.

---

## 🎨 Frontend Search (React)

In the React frontend, the search is managed by:

1. **Search Input Field**: A text field for users to type the search term.
2. **State Management**: The search query is stored in the `searchText` state.
3. **`useEffect` Hook**: The search query is sent to the backend API whenever the search term or other parameters change.

---

### **Search Input UI**

```jsx
<form>
  <label className="input max-w-[300px] w-[300px] input-secondary">
    <svg
      className="h-[1em] opacity-50"
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
    >
      <g
        strokeLinejoin="round"
        strokeLinecap="round"
        strokeWidth="2.5"
        fill="none"
        stroke="currentColor"
      >
        <circle cx="11" cy="11" r="8"></circle>
        <path d="m21 21-4.3-4.3"></path>
      </g>
    </svg>
    <input
      onChange={handelSearch}
      type="search"
      className=""
      placeholder="Search Apps"
    />
  </label>
</form>
```

### ✔ Explanation of Search Input

* **`onChange` Handler**: Every time the user types something, the `handelSearch` function is called to update the `searchText` state.
* **Debounced Search**: This search can be enhanced with **debouncing** to reduce the number of API requests while typing (optional).

---

### **Search State and `useEffect` Hook**

```javascript
const handelSearch = (e) => {
  setSearchtext(e.target.value); // Update search query
};

useEffect(() => {
  fetch(
    `http://localhost:5000/apps?limit=${limit}&skip=${
      currentPage * limit
    }&sort=${sort}&order=${order}&search=${searchText}`
  )
    .then((res) => res.json())
    .then((data) => {
      setApps(data.apps);
      setTotalapp(data.total);

      const pages = Math.ceil(data.total / limit);
      setTotalpages(pages);
    });
}, [currentPage, order, sort, searchText]); // Trigger search when any dependency changes
```

### ✔ When does this run?

* **When the search text (`searchText`) changes**
* **When the sorting, pagination, or other filters change**

---

## 📌 Search Flow Summary

1. **User inputs a search term** (e.g., `calculator`).
2. **Search query (`searchText`)** is updated in the React state.
3. **`useEffect`** sends a request to the backend:

   ```
   GET /apps?search=calculator&sort=size&order=asc
   ```
4. Backend uses **regex** to filter apps based on the title.
5. **Paginated and sorted** search results are returned to React and displayed.

---

# 🎯 Final Result

* The **search bar** allows users to filter apps based on their title.
* The search query is **case-insensitive** and uses **regular expressions** for flexibility.
* Combined with **pagination** and **sorting**, the user experience is optimized for large datasets.

---

# 🔧 Optional Enhancements

* **Debounced search**: Introduce a delay before making a request after the user stops typing (useful for preventing multiple API calls while typing).
* **Clear Search**: Add a button to clear the search input and show all results.

```
