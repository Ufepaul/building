1. Component for Fetching Data
Here, we'll create an App component that fetches data, maintains loading and error states, and then passes the retrieved data to a reusable list component.
// App.js
import React, { useEffect, useState } from 'react';
import ListComponent from './ListComponent';

function App() {
  const [items, setItems] = useState([]);       // Store API data here
  const [loading, setLoading] = useState(true);   // Track loading status
  const [error, setError] = useState(null);       // Track errors

  useEffect(() => {
    // Replace the URL below with any public API you wish to use
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(response => {
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        return response.json();
      })
      .then(data => {
        setItems(data);
        setLoading(false);
      })
      .catch(err => {
        console.error("Error fetching data:", err);
        setError(err);
        setLoading(false);
      });
  }, []);

  if (loading) {
    return <p>Loading data...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }

  return (
    <div>
      <h1>User List</h1>
      {/* Pass a renderItem callback for custom rendering */}
      <ListComponent
        items={items}
        renderItem={(item) => (
          <div style={{border: '1px solid #ccc', padding: '10px', marginBottom: '10px'}}>
            <h2>{item.name}</h2>
            <p>Email: {item.email}</p>
            <p>Phone: {item.phone}</p>
          </div>
        )}
      />
    </div>
  );
}

export default App;


Explanation:
- The useEffect hook fetches data once the component mounts.
- We handle errors by setting an error state, which we check before rendering the data.
- When fetching is complete, the items state holds the data which is then passed to our reusable ListComponent.

2. Creating a Reusable List Component
The ListComponent is designed to be flexible. It accepts an array of items and a renderItem function. If no custom rendering is provided, it will default to a simple output.
// ListComponent.js
import React from 'react';

function ListComponent({ items, renderItem }) {
  return (
    <div>
      {items.map(item => (
        <div key={item.id}>
          {renderItem ? renderItem(item) : <p>{JSON.stringify(item)}</p>}
        </div>
      ))}
    </div>
  );
}

export default ListComponent;


Explanation:
- The component maps over the items array.
- For each item, it uses the renderItem callback (if provided) to determine the rendered output. This makes your list component highly reusable—different consumers can render items differently.
- If renderItem is missing, it falls back to displaying the entire item as a JSON string.

Further Enhancements
- Search & Filter Functionality:
To allow users to quickly find items, you might introduce a search input in your main component. Use useState to capture the search query and filter the items array accordingly before passing it to ListComponent.
- Pagination:
For large datasets, pagination is essential. You can keep track of the current page and limit the number of items per page. Create helper functions to move between pages and use array slicing to render only a subset of the data.
- Styling & Customization:
Whether you choose traditional CSS, a preprocessor, or a UI framework like Material UI or TailwindCSS, adding style can significantly improve the user experience. Consider animations or transitions for loading states to make the interface more engaging.

This structure should set you on a strong path to completing your project. It separates concerns while making your code modular and maintainable. Do you feel ready to integrate search or pagination next, or would you like to explore additional customization options for the list component? Let’s dive deeper into whichever feature excites you most!
