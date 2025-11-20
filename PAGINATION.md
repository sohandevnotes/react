
Here is a **clean, beautiful, fully-explained Markdown (.md) documentation** for your **React Pagination + Backend Pagination API**.

You can copy & paste directly into a `.md` file.

---

# 📄 **React + Node.js Pagination Documentation**

This document explains how the **backend pagination API** and the **React frontend pagination UI** work together to fetch paginated data seamlessly.

---

# 🚀 **Backend Pagination API**

### **Endpoint:**

```
GET /apps?limit=10&skip=0
```

### **Description:**

This API returns **paginated app data**, excluding heavy fields (`description`, `ratings`).
It also returns the **total count**, allowing the frontend to calculate total pages.

---

## ✅ **Backend Code**

```javascript
app.get("/apps", async (req, res) => {
  try {
    const { limit = 0, skip = 0 } = req.query;

    const apps = await appsCollection
      .find()
      .limit(parseInt(limit))
      .skip(parseInt(skip))
      .project({ description: 0, ratings: 0 })
      .toArray();

    const count = await appsCollection.countDocuments();

    res.send({ apps, total: count });
  } catch (error) {
    console.log(error);
    res.status(500).json({ error: "Internal Server Error" });
  }
});
```

---

## 📝 **How It Works**

| Query Param | Meaning                  | Example                          |
| ----------- | ------------------------ | -------------------------------- |
| `limit`     | Number of items per page | `?limit=10`                      |
| `skip`      | How many items to skip   | `?skip=20` (skip first 20 items) |

### 📌 Example Request

```
/apps?limit=10&skip=30
```

➡ Returns **10 apps**, starting from the **31st** app.

### 📌 Example Response

```json
{
  "apps": [ ...10 items... ],
  "total": 128
}
```

---

# 🎨 **Frontend React Pagination**

This is the **React page that fetches paginated data** and renders a page selector (Prev / Next Buttons + Page Numbers).

---

## 🧩 **State Variables**

| State         | Purpose                                              |
| ------------- | ---------------------------------------------------- |
| `apps`        | Stores the apps fetched from backend                 |
| `totalapps`   | Total number of apps from the backend                |
| `totalpages`  | Total pages → computed as `Math.ceil(total / limit)` |
| `currentPage` | Current page index                                   |
| `limit`       | Items per page (set to 10)                           |

---

## 🧠 **Pagination Logic**

Backend Request:

```
/apps?limit=10&skip=currentPage * 10
```

If `currentPage = 3` →
`skip = 3 × 10 = 30`

---

## ✅ **Frontend Code**

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

  useEffect(() => {
    fetch(
      `http://localhost:5000/apps?limit=${limit}&skip=${currentPage * limit}`
    )
      .then((res) => res.json())
      .then((data) => {
        setApps(data.apps);
        setTotalapp(data.total);
        setTotalpages(Math.ceil(data.total / limit));
      });
  }, [currentPage]);

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

      {/* Count */}
      <div className="w-11/12 mx-auto mt-10">
        <h2 className="text-lg underline font-bold">
          ({totalapps}) Apps Found
        </h2>
      </div>

      {/* Apps Grid */}
      <div className="w-11/12 mx-auto grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 my-10 gap-5">
        {apps.length === 0 ? (
          <div className="col-span-full text-center py-10 space-y-10">
            <h2 className="text-6xl font-semibold opacity-60">No Apps Found</h2>
            <button className="btn btn-primary">Show All Apps</button>
          </div>
        ) : (
          apps.map((app) => <AppCard key={app.id} app={app} />)
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
            className={`btn ${i === currentPage && "btn-primary"}`}
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

---

# 📘 **Flow Summary**

### ✔ Backend

* Applies **limit & skip**
* Removes heavy fields using `.project()`
* Returns **apps + total count**

### ✔ Frontend

* Calculates pages using `total / limit`
* Uses `currentPage * limit` to compute skip
* Renders pagination buttons dynamically
* Updates page using `setCurrentPage()`

---

# 🎯 Final Result

A fully functional **server-side pagination system** where:

* API handles efficient database pagination
* React UI displays real page numbers
* Users can navigate with **Prev / Next / Numbered pages**
* Clean, fast, scalable pagination system

---

If you want, I can also make a **diagram**, **flowchart**, or **animated GIF explanation** for this pagination system.
