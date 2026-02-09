# Checkpoint 4.1: Interactive Data-Driven UIs

Build a **Product Browser** application that fetches product data from the [DummyJSON Products API](https://dummyjson.com/products), renders it to the page, and lets users click products to view details and search for products using a form.

## Getting Started

A Vite project has been set up for you with the HTML, CSS, and configuration files.

```sh
npm i
npm run dev
```

**Provided files (do not modify):**
- `index.html` — page structure with a search form, product list, and details section
- `styles.css` — basic layout and styling
- `package.json` — Vite dev server

**Files you complete:**
- `src/fetch-helpers.js`
- `src/dom-helpers.js`
- `src/main.js` — wire everything together

## API Reference

| Endpoint                                          | Description                                                                                       |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `https://dummyjson.com/products?limit=12`         | Get 12 products. Response: `{ products: [...], total, skip, limit }`                              |
| `https://dummyjson.com/products/{id}`             | Get a single product by ID. Response: `{ id, title, price, description, rating, thumbnail, ... }` |
| `https://dummyjson.com/products/search?q={query}` | Search products. Response: `{ products: [...], total, skip, limit }`                              |

## Requirements

### Part 1: Render All Products

When the page loads, the app should fetch 12 products from the API and render them as a list of cards.

First, in `src/fetch-helpers.js` create and export a `getProducts()` function that:
- Invokes `fetch()` to get 12 products from the `https://dummyjson.com/products?limit=12` endpoint
- Handles the resolved/rejected promise with `.then()` and `.catch()`
- Checks `response.ok` and throws an error if the response is not ok
- Returns a `{ data, error }` object for both resolved and rejected promises.

Then, in `src/dom-helpers.js` create and export a `renderProducts(products)` function that:
- Clears the existing `#products-list` content
- Updates `#product-count` with the number of products
- For each product, creates an `li` with:
  - An `img` (using `product.thumbnail` as the source and `product.title` as the alt text)
  - An `h3` with the product title
  - A `p` with the price
- Stores the product `id` on the `li` using a `data-` attribute
- Appends each `li` to `#products-list`

Lastly, in `src/main.js`, combine your function to fetch and render products on page load

- Import `getProducts` from `fetch-helpers.js` and `renderProducts` from `dom-helpers.js`
- Call `getProducts()` and chain `.then()` to render the products
- If the fetch fails, display the `error.message` value in the `#error-message` paragraph element.

### Part 2: Product Details with Event Delegation

When a user clicks on a product card, the app should fetch that product's full details and display them.

First, in `src/fetch-helpers.js` add and export a `getProductById(id)` function that:
- Invokes `fetch()` to get a single product from the `https://dummyjson.com/products/{id}` endpoint
- Handles the resolved/rejected promise with `.then()` and `.catch()`
- Checks `response.ok` and throws an error if the response is not ok
- Returns a `{ data, error }` object for both resolved and rejected promises.

Then, in `src/dom-helpers.js` add and export a `renderProductDetails(product)` function that:
- Clears the `#product-details` section and makes it visible (removes the `hidden` attribute)
- Displays the product's title, thumbnail, price, description, and using the existing HTML elements

Lastly, in `src/main.js`, add a click handler using event delegation:
- Add a single `click` event listener on `#products-list` (not on each `li`)
- Use `event.target.closest('li')` to find the clicked product card
- Read the product ID from the element's `dataset`
- Fetch the product by ID and chain `.then()` to render its details
- If the fetch fails, display the `error.message` value in the `#error-message` paragraph element

### Part 3: Search Form with Async/Await

Add search functionality using `async`/`await` syntax instead of `.then()`/`.catch()`.

First, in `src/fetch-helpers.js` add and export a `searchProducts(query)` function that:
- Invokes `fetch()` with the `https://dummyjson.com/products/search?q={query}` endpoint
- Uses `async`/`await` with `try`/`catch` (instead of `.then()`/`.catch()`)
- Checks `response.ok` and throws an error if the response is not ok
- Returns a `{ data, error }` object for both resolved and rejected promises.

Then, in `src/main.js`, add a form submit handler:
- Add a `submit` event listener to `#search-form`
- Prevent the default form submission behavior
- Extract the search query from the form
- Use `await searchProducts()` to fetch products matching the query from the form
- Re-render the product list with the search results using `renderProducts()`
- If the fetch fails, display the `error.message` value in the `#error-message` paragraph element
- Reset the form after a successful search

---

## Grading Rubric

### ES Modules

- [ ] Code is organized across `fetch-helpers.js`, `dom-helpers.js`, and `main.js`
- [ ] Functions are exported using named exports
- [ ] Functions are imported using named imports with the `.js` extension

### Fetching with `.then()` and `.catch()`

- [ ] `getProducts` and `getProductById` use `fetch()` with `.then()` and `.catch()`
- [ ] `response.ok` is checked and an error is thrown when it is `false`
- [ ] `response.json()` is returned from the first `.then()` callback
- [ ] The `{ data, error }` return pattern is used

### Fetching with `async`/`await`

- [ ] `searchProducts` is an `async` function
- [ ] `fetch()` and `response.json()` are called with `await`
- [ ] `response.ok` is checked and an error is thrown when it is `false`
- [ ] `try`/`catch` is used for error handling
- [ ] The `{ data, error }` return pattern is used

### Rendering Product Data

- [ ] Product `li` elements are created using `document.createElement`
- [ ] Each `li` contains an `img`, `h3` (title), and `p` (price)
- [ ] The product `id` is stored on the `li` using a `data-` attribute (`dataset`)
- [ ] Products are appended to `#products-list`
- [ ] `#product-count` is updated with the number of products
- [ ] Clicking a product renders its details (title, image, price, description, rating) in `#product-details`

### Event Delegation

- [ ] A single event listener on `#products-list` handles clicks on individual products
- [ ] `event.target.closest('li')` is used to find the clicked `li`
- [ ] The product ID is read from the clicked element's `dataset`

### Form Handling

- [ ] The `submit` event on `#search-form` is handled
- [ ] The callback is an `async` function
- [ ] `event.preventDefault()` is called
- [ ] The search query data is extracted from the form using either `FormData` or `form.elements`
- [ ] `await` is used to fetch products matching the query
- [ ] The product list is re-rendered with the search results
- [ ] The form is reset after submission

### Error Handling

- [ ] Error messages are displayed in `#error-message` when fetches fail
- [ ] Previous errors are cleared on new operations
