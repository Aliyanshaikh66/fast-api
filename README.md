# FastAPI CRUD Application

This project demonstrates a basic CRUD (Create, Read, Update, Delete) application using **FastAPI**. It allows you to perform CRUD operations on an SQLite database using **SQLAlchemy** for ORM-based interaction.

## Features

- **Create**: Add new records to the database.
- **Read**: Retrieve records from the database.
- **Update**: Modify existing records.
- **Delete**: Remove records from the database.

## Requirements

- Python 3.7+
- FastAPI
- SQLAlchemy (for database interaction)
- Uvicorn (ASGI server)

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/your-username/fastapi-crud.git
cd fastapi-crud
```

### 2. Create a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate   # On Windows, use `venv\Scripts\activate`
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the application

```bash
uvicorn app.main:app --reload
```

This will start the FastAPI application on `http://127.0.0.1:8000`.

## API Endpoints

### 1. Create a new record

**Endpoint**: `POST /items/`

**Request Body**:

```json
{
  "name": "Sample Item",
  "description": "A brief description of the item",
  "price": 10.0
}
```

**Response**:

```json
{
  "id": 1,
  "name": "Sample Item",
  "description": "A brief description of the item",
  "price": 10.0
}
```

---

### 2. Get all records

**Endpoint**: `GET /items/`

**Response**:

```json
[
  {
    "id": 1,
    "name": "Sample Item",
    "description": "A brief description of the item",
    "price": 10.0
  },
  {
    "id": 2,
    "name": "Another Item",
    "description": "Another item description",
    "price": 20.0
  }
]
```

---

### 3. Get a record by ID

**Endpoint**: `GET /items/{id}`

**Response**:

```json
{
  "id": 1,
  "name": "Sample Item",
  "description": "A brief description of the item",
  "price": 10.0
}
```

---

### 4. Update a record

**Endpoint**: `PUT /items/{id}`

**Request Body**:

```json
{
  "name": "Updated Item",
  "description": "Updated description",
  "price": 15.0
}
```

**Response**:

```json
{
  "id": 1,
  "name": "Updated Item",
  "description": "Updated description",
  "price": 15.0
}
```

---

### 5. Delete a record

**Endpoint**: `DELETE /items/{id}`

**Response**:

```json
{
  "message": "Item deleted successfully"
}
```

## Database Configuration

This application uses **SQLite** for simplicity, but you can easily modify the app to use **PostgreSQL**, **MySQL**, or other databases by changing the connection URL in `config.py`.

### Example SQLite Configuration (`config.py`):

```python
DATABASE_URL = "sqlite:///./test.db"
```

### SQLAlchemy Database Setup (`database.py`):

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite:///./test.db"

engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```

### Example Model (`models.py`):

```python
from sqlalchemy import Column, Integer, String, Float
from app.database import Base

class Item(Base):
    __tablename__ = "items"

    id = Column(Integer, primary_key=True, index=True)
    name = Column(String, index=True)
    description = Column(String)
    price = Column(Float)
```

### Example Dependency to get DB session (`dependencies.py`):

```python
from sqlalchemy.orm import Session
from app.database import SessionLocal

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

## Running Tests

To run the tests, use the following command:

```bash
pytest
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


This `README.md` file is well-structured and provides all the necessary details to set up, run, and test the FastAPI CRUD application. You can copy and use it directly for your project.
