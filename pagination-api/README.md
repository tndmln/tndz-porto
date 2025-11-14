# Pagination API

A Spring Boot application that provides paginated user data with search functionality, fetching data from an external API with caching capabilities.

## Features

- **Paginated User Listing** - Retrieve users with page/size parameters
- **User Search** - Search users by first name or last name
- **Caching** - In-memory caching to reduce external API calls
- **Input Validation** - Proper validation for pagination parameters
- **API Documentation** - Interactive Swagger UI
- **Error Handling** - Comprehensive exception handling

## Prerequisites

- Java 17 or higher
- Maven 3.6 or higher

## How to Run

### 1. Clone and Build
```bash
git clone https://github.com/tndmln/tndz-porto.git
cd pagination-api
mvn clean package
```

### 2. Run the Application
```bash
mvn spring-boot:run
```

The application will start on `http://localhost:8080`

### 3. Access API Documentation
Once running, access the Swagger UI at:
```
http://localhost:8080/swagger-ui.html
```

## API Endpoints

### Get Paginated Users
```
GET /api/users?page=1&size=10
```

**Response:**
```json
{
  "page": 1,
  "size": 10,
  "totalItems": 100,
  "totalPages": 10,
  "data": [
    {
      "id": 1,
      "firstName": "Tandi",
      "lastName": "Maulana",
      "age": 24,
      "email": "tandi.maulan@example.com"
    }
  ]
}
```

### Search Users by Name
```
GET /api/users/search?name=tandi&page=1&size=5
```

**Response:**
```json
{
  "page": 1,
  "size": 5,
  "totalItems": 3,
  "totalPages": 1,
  "data": [
    {
      "id": 1,
      "firstName": "Tandi",
      "lastName": "Maulana",
      "age": 24,
      "email": "tandi.maulan@example.com"
    }
  ]
}
```

## Configuration

Key configuration properties in `application.yml`:

```yaml
external:
  api:
    url: https://dummyjson.com/users  # External API endpoint

app:
  cache:
    duration-ms: 300000  # Cache duration (5 minutes)
  pagination:
    max-size: 100        # Maximum page size allowed
```

## Assumptions

1. **External API Reliability**: The external API (`dummyjson.com`) is generally available and returns consistent data structure
2. **Data Volume**: The dataset is small enough to cache in memory (typically < 1000 users)
3. **Search Requirements**: Case-insensitive partial matching on first name and last name is sufficient
4. **Caching Strategy**: 5-minute cache duration balances freshness vs performance
5. **Error Handling**: Network timeouts and external API errors are handled gracefully
6. **Pagination Limits**: Maximum page size of 100 to prevent excessive resource usage

## Error Handling

The API returns appropriate HTTP status codes:

- `400 Bad Request` - Invalid pagination parameters
- `500 Internal Server Error` - External API or network issues
- `200 OK` - Successful requests

## Testing

Run the test suite:
```bash
mvn test
```

## Project Structure

```
src/main/java/com/example/pagination/
├── controllers/     # REST controllers
├── services/       # Business logic
├── models/         # Data models
├── dto/           # Data transfer objects
├── exceptions/    # Custom exceptions
└── config/        # Configuration classes
```
