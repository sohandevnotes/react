# React + Node.js Pagination and Sorting Documentation

## Introduction

This document describes how to implement pagination and sorting functionality in a **React** frontend with a **Node.js** backend. The setup includes fetching paginated data and allowing sorting of the results (by fields such as rating, size, and downloads).

---

## Backend API for Pagination and Sorting

### **Endpoint**

```

GET /apps?limit=10&skip=0&sort=size&order=desc

```

### **Query Parameters:**

| Parameter | Description                                      | Example           |
| --------- | ------------------------------------------------ | ----------------- |
| `limit`   | Number of items to fetch per page.               | `?limit=10`       |
| `skip`    | Number of items to skip (for pagination).        | `?skip=20`        |
| `sort`    | Field to sort by (e.g., `rating`, `size`, etc.). | `?sort=size`      |
| `order`   | Sorting order (`asc` or `desc`).                 | `?order=desc`     |

### **Example Request**

```

GET /apps?limit=10&skip=30&sort=size&order=asc

````

This fetches **10 apps**, starting from the **31st app**, sorted by **size** in **ascending** order.

### **Example Response**

```json
{
  "apps": [ ...10 items... ],
  "total": 128
}
````

### **Backend Code Example**

```javascript
app.get("/apps", async (req, res) => {
  try {
    const { limit = 0, skip = 0, sort = "size", order = "desc" } = req.query;

    // Set sorting option based on query params
    const sortOption = {};
    sortOption[sort || "size"] = order === "asc" ? 1 : -1;

    const apps = await appsCollection
      .find()
      .sort(sortOption)  // Sorting by selected field
      .limit(parseInt(limit))  // Pagination limit
      .skip(parseInt(skip))  // Pagination offset
      .project({ description: 0, ratings: 0 })  // Exclude heavy fields
      .toArray();

    const count = await appsCollection.countDocuments(); // Total count for pagination

    res.send({ apps, total: count });
  } catch (error) {
    res.status(500).json({ error: "Internal Server Error" });
  }
});
```

---

## Frontend React Code with Pagination and Sorting

This section explains the **React component** that fetches paginated and sorted data from the backend and displays it to the user with sorting and pagination options.

### **State Variables**

| State         | Purpose                                                         |
| ------------- | --------------------------------------------------------------- |
| `apps`        | Stores the apps fetched from the backend.                       |
| `totalapps`   | Total number of apps fetched from the backend.                  |
| `totalpages`  | Total number of pages, calculated from `totalapps` and `limit`. |
| `currentPage` | Current page index to determine the subset of data.             |
| `limit`       | Number of apps per page (set to 10 in this example).            |
| `sort`        | Field to sort by (e.g., `size`, `rating`, etc.).                |
| `order`       | Sorting order (`asc` or `desc`).                                |

### **Sorting Select UI (`handleSelect`)**

The sorting is handled by a dropdown (`<select>`), where the user can choose the sorting field and order (ascending or descending). The selected value is then parsed and used to update the `sort` and `order` state.

```javascript
const handleSelect = (e) => {
  const sortText = e.target.value;
  setSort(sortText.split("-")[0]);
  setOrder(sortText.split("-")[1]);
  setCurrentPage(0); // Reset page on sort change
};
```

### **Fetching Data with `useEffect`**

The `useEffect` hook fetches the data based on the current `currentPage`, `sort`, and `order`. Whenever the user changes the sorting option or navigates to a different page, a new request is made to the backend.

```javascript
useEffect(() => {
  fetch(
    `http://localhost:5000/apps?limit=${limit}&skip=${currentPage * limit}&sort=${sort}&order=${order}`
  )
    .then((res) => res.json())
    .then((data) => {
      setApps(data.apps);
      setTotalapp(data.total);
      setTotalpages(Math.ceil(data.total / limit));
    });
}, [currentPage, order, sort]);
```

### **Frontend Pagination UI**

Pagination is implemented with buttons to navigate between pages. The `Prev` and `Next` buttons adjust the `currentPage`, while the numbered buttons allow jumping to a specific page.

```javascript
<div className="flex justify-center flex-wrap gap-3 py-5">
  {currentPage > 0 && (
    <button
      onClick={() => setCurrentPage(currentPage - 1)}
      className="btn"
    >
      Prev
    </button>
  )}

  {[...Array(totalpages).keys()].map((i) => (
    <button
      onClick={() => setCurrentPage(i)}
      className={`btn ${i == currentPage && "btn-primary"}`}
    >
      {i}
    </button>
  ))}

  {currentPage < totalpages - 1 && (
    <button
      onClick={() => setCurrentPage(currentPage + 1)}
      className="btn"
    >
      Next
    </button>
  )}
</div>
```

### **Full Frontend Code (AllAppsPage Component)**

```javascript
import { DiVisualstudio } from "react-icons/di";
import AppCard from "../ui/AppCard";
import { useEffect, useState } from "react";

const AllAppsPage = () => {
  const [apps, setApps] = useState([]);
  const [totalapps, setTotalapp] = useState(0);
  const [totalpages, setTotalpages] = useState(0);
  const [currentPage, setCurrentPage] = useState(0);
  const limit = 10;
  const [sort, setSort] = useState("size");
  const [order, setOrder] = useState("desc");
  const [loading, setLoading] = useState(false);

  const handleSelect = (e) => {
    const sortText = e.target.value;
    setSort(sortText.split("-")[0]);
    setOrder(sortText.split("-")[1]);
    setCurrentPage(0); // Reset page on sort change
  };

  useEffect(() => {
    setLoading(true);
    fetch(
      `http://localhost:5000/apps?limit=${limit}&skip=${currentPage * limit}&sort=${sort}&order=${order}`
    )
      .then((res) => res.json())
      .then((data) => {
        setApps(data.apps);
        setTotalapp(data.total);
        setTotalpages(Math.ceil(data.total / limit));
        setLoading(false);
      });
  }, [currentPage, order, sort]);

  if (loading) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <title>All Apps | Hero Apps</title>
      {/* Header */}
      <div className="py-16">
        <h2 className="text-4xl font-bold text-center text-primary flex justify-center gap-3">
          Our All Applications
          <DiVisualstudio size={48} className="text-secondary" />
        </h2>
        <p className="text-center text-gray-400">
          Explore All Apps on the Market developed by us. We code for Millions
        </p>
      </div>

      {/* Search and Count */}
      <div className="w-11/12 mx-auto flex flex-col-reverse lg:flex-row gap-5 items-start justify-between lg:items-end mt-10">
        <div>
          <h2 className="text-lg underline font-bold">
            ({totalapps}) Apps Found
          </h2>
        </div>

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
            <input type="search" className="" placeholder="Search Apps" />
          </label>
        </form>

        {/* Sorting UI */}
        <div>
          <select
            onChange={handleSelect}
            className="select bg-white"
            aria-label="Sort apps by rating, size, or downloads"
          >
            <option selected disabled>
              Sort by
            </option>
            <option value="rating-desc">Ratings: High - Low</option>
            <option value="rating-asc">Ratings: Low - High</option>
            <option value="size-desc">Size: High -Low</option> <option value="size-asc">Size: Low - High</option> <option value="downloads-desc">Downloads: High - Low</option> <option value="downloads-asc">Downloads: Low - High</option> </select> </div> </div>

  {/* Apps Grid */}
  <div className="w-11/12 mx-auto grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 my-10 gap-5">
    {apps.length === 0 ? (
      <div className="col-span-full text-center py-10 space-y-10">
        <h2 className="text-6xl font-semibold opacity-60">No Apps Found</h2>
        <button className="btn btn-primary">Show All Apps</button>
      </div>
    ) : (
      apps.map((app) => <AppCard key={app.id} app={app}></AppCard>)
    )}
  </div>

  {/* Pagination */}
  <div className="flex justify-center flex-wrap gap-3 py-5">
    {currentPage > 0 && (
      <button
        onClick={() => setCurrentPage(currentPage - 1)}
        className="btn"
      >
        Prev
      </button>
    )}

    {[...Array(totalpages).keys()].map((i) => (
      <button
        onClick={() => setCurrentPage(i)}
        className={`btn ${i == currentPage && "btn-primary"}`}
      >
        {i}
      </button>
    ))}

    {currentPage < totalpages - 1 && (
      <button
        onClick={() => setCurrentPage(currentPage + 1)}
        className="btn"
      >
        Next
      </button>
    )}
  </div>
</div>
);
};

export default AllAppsPage;

```
