📦 Products Table with Pagination, Sorting & Filtering (Vanilla Vue)

This is a single-file Vue component that displays a products table with:

✅ Pagination (selectable rows per page)
✅ Global and per-column filtering
✅ Sorting on every column
✅ Responsive layout (no horizontal scroll)
✅ No third-party libraries (pure Vue + CSS)

The component uses the API:

https://dummyjson.com/products?limit=100

📋 Features
Feature	Description
Pagination	Custom pagination with page size selector
Sorting	Click table header to sort asc/desc
Filtering	Global search + individual column filters
Responsive	Text wraps, no horizontal scroll
Lightweight	No external libraries
🧱 Installation

Ensure you have Vue 3 CLI or Vite setup.

Add the provided Vue file (e.g., ProductsTable.vue) into your project.

Import and use the component.

npm install
npm run dev

📁 File Structure
src/
├── components/
│   └── ProductsTable.vue
├── App.vue
└── main.js

🔌 API Used
const fetchProducts = async () => {
  try {
    const response = await fetch("https://dummyjson.com/products?limit=100");
    const data = await response.json();
    products = data.products;
  } catch (e) {
    console.error(e);
  }
};

🧠 How It Works (Summary)

Fetching Data
Fetch products from API and cache them in memory.

Filtering

Global search on title.

Individual per-column search inputs.

Sorting

Click a header to sort by that column.

Click again to toggle ascending/descending.

Pagination

Select page size from a dropdown.

Navigate with Prev/Next buttons.

Responsive Layout

Table fills width and text wraps inside cells.

No horizontal scrollbar unless absolutely needed.

🧱 Component Properties
Name	Type	Default	Description
currentPage	Number	1	Active page number
perPage	Number	10	Rows per page
perPageOptions	Array	[5,10,15,20,50]	Dropdown options
searchText	String	""	Global search term
columnFilters	Object	{…}	Per-column filters
sortKey	String	""	Active sort field
sortOrder	String	"asc"	Sort direction
⏳ Screenshots

(Add your own screenshots here if desired)

🏷️ Usage Example
<template>
  <ProductsTable />
</template>

<script>
import ProductsTable from "@/components/ProductsTable.vue";

export default {
  components: {
    ProductsTable
  }
};
</script>

💡 Tips

You can extend caching to localStorage if needed.

Images, nested fields or custom formats can be added easily.

Add reset filters/sort buttons for better UX.

📦 Dependencies

✔ Vue 3
❌ No UI framework (like Bootstrap/Tailwind)
❌ No external table plugin

🔐 License

This project is open-source and available for reuse.
