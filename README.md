Product Inventory API

Simple REST API built with Node.js and Express to serve a static list of products from `data/products.js`.

## Features
- **Express server** with JSON middleware
- **Environment variables** via `dotenv`
- **Products endpoint** returning predefined inventory

## Tech Stack
- **Runtime:** Node.js
- **Framework:** Express
- **Config:** dotenv
- **Dev tooling:** nodemon (optional)

## Project Structure
```
productInventory-main/
├─ data/
│  └─ products.js        # Static product list (exported array)
├─ server.js             # Express app and routes
├─ package.json          # Scripts and dependencies
├─ package-lock.json
└─ .gitignore
```

## Getting Started

### Prerequisites
- Node.js 18+ (recommended)
- npm 9+

### 1) Install dependencies
```bash
npm install
```

### 2) Configure environment
Create a `.env` file in the project root with a port value:
```env
PORT=3000
```

> Important: `server.js` reads `process.env.PORT`. If `PORT` is missing, the server may fail to start or listen on an undefined port.

### 3) Run the server
- Production/start mode:
```bash
npm start
```
- Dev mode with auto-reload (nodemon):
```bash
npx nodemon server.js
```

> Note: The existing `dev` script in `package.json` points to `serverapp.js`, which does not exist. You can fix it like so:
> ```json
> {
>   "scripts": {
>     "dev": "nodemon server.js"
>   }
> }
> ```

The server will start on `http://localhost:<PORT>` (e.g., `http://localhost:3000`).

## API Endpoints

### GET `/`
Health check for the API.
- Response: `text/plain`
```
Product Inventory API is running
```

### GET `/products`
Returns all products from `data/products.js`.
- Success `200`:
```json
{
  "message": "Products retrieved successfully",
  "data": [
    { "id": 1, "name": "Samsung S22 Ultra", "price": 600, "InStock": 10 },
    { "id": 2, "name": "Samsung Galaxy A54", "price": 400, "InStock": 15 }
  ]
}
```
- Not Found `404` (when no products exist):
```json
{
  "message": "No products found",
  "data": []
}
```

### GET `/products/:id`
Returns a single product by id.
- Success `200`:
```json
{
  "message": "Product retrieved successfully",
  "data": { "id": 1, "name": "Samsung S22 Ultra", "price": 600, "InStock": 10 }
}
```
- Not Found `404`:
```json
{ "message": "Product not found", "data": null }
```

### POST `/products`
Create a new product. Expects JSON body: `{ name: string, price: number, InStock: number }`.
- Created `201`:
```json
{
  "message": "Product created successfully",
  "data": { "id": 16, "name": "New Phone", "price": 499, "InStock": 7 }
}
```
- Bad Request `400` (invalid payload):
```json
{ "message": "Invalid payload. Expected { name: string, price: number, InStock: number }", "data": null }
```

### PUT `/products/:id`
Replace an existing product. Requires full object: `{ name, price, InStock }`.
- Success `200`:
```json
{
  "message": "Product replaced successfully",
  "data": { "id": 1, "name": "Updated Name", "price": 640, "InStock": 12 }
}
```
- Not Found `404` or Bad Request `400` (invalid payload).

### PATCH `/products/:id`
Partially update an existing product. Allowed fields: `name`, `price`, `InStock`.
- Success `200`:
```json
{
  "message": "Product updated successfully",
  "data": { "id": 1, "name": "Samsung S22 Ultra", "price": 620, "InStock": 8 }
}
```
- Not Found `404` or Bad Request `400` (invalid field/types or empty body):
```json
{ "message": "Product not found", "data": null }
```

### DELETE `/products/:id`
Delete a product by id.
- Success `200` (returns deleted entity):
```json
{
  "message": "Product deleted successfully",
  "data": { "id": 1, "name": "Samsung S22 Ultra", "price": 600, "InStock": 10 }
}
```
- Not Found `404`:
```json
{ "message": "Product not found", "data": null }
```

### Sample curl
```bash
curl http://localhost:3000/
curl http://localhost:3000/products
curl http://localhost:3000/products/1

# create
curl -X POST http://localhost:3000/products \
  -H "Content-Type: application/json" \
  -d '{"name":"New Phone","price":499,"InStock":7}'

# put (replace)
curl -X PUT http://localhost:3000/products/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Updated Name","price":640,"InStock":12}'

# patch
curl -X PATCH http://localhost:3000/products/1 \
  -H "Content-Type: application/json" \
  -d '{"price":620,"InStock":8}'

# delete
curl -X DELETE http://localhost:3000/products/1
```

## Data Model
Each product in `data/products.js` has the shape:
```json
{
  "id": number,
  "name": string,
  "price": number,
  "InStock": number
}
```

## Scripts
- `npm start` — runs `node server.js`
- `npm run dev` — recommended to update to `nodemon server.js`

## License
ISC

## Notes / Future Improvements
- Add pagination, filtering, and sorting for `/products`
- Persist data via a database (SQLite/PostgreSQL/MongoDB)
- Input validation and error handling
- Tests (unit/integration) and CI
