# Примеры использования GraphQL API client-info

Ниже несколько типовых запросов и ожидаемых структур ответов для спроектированной схемы.

---

## 1. Получить только базовую информацию о клиенте

Аналог REST `GET /clients/{id}`.

### Запрос

```graphql
query GetClientBasic {
  client(id: "123") {
    id
    name
    age
  }
}
```

### Ответ (пример) 
```graphql
{
  "data": {
    "client": {
      "id": "123",
      "name": "Иван Иванов",
      "age": 35
    }
  }
}
```
## 2. Клиент + документы + родственники одним запросом

В REST это были бы три запроса:
GET /clients/{id}, GET /clients/{id}/documents, GET /clients/{id}/relatives.

### Запрос

```graphql
query GetClientFullCard {
  client(id: "123") {
    id
    name
    age
    documents {
      id
      type
      number
      expiryDate
    }
    relatives {
      id
      relationType
      name
      age
    }
  }
}
```

### Ответ (пример)
```graphql
{
  "data": {
    "client": {
      "id": "123",
      "name": "Иван Иванов",
      "age": 35,
      "documents": [
        {
          "id": "doc-1",
          "type": "PASSPORT",
          "number": "1234 567890",
          "expiryDate": "2030-01-01"
        },
        {
          "id": "doc-2",
          "type": "DRIVER_LICENSE",
          "number": "77 11 222333",
          "expiryDate": "2028-05-10"
        }
      ],
      "relatives": [
        {
          "id": "rel-1",
          "relationType": "SPOUSE",
          "name": "Мария Иванова",
          "age": 34
        },
        {
          "id": "rel-2",
          "relationType": "CHILD",
          "name": "Павел Иванов",
          "age": 6
        }
      ]
    }
  }
}
```

## 3. Только документы клиента (без данных о клиенте)
Аналог REST GET /clients/{id}/documents.

### Запрос

```graphql
query GetClientDocuments {
  clientDocuments(clientId: "123") {
    id
    type
    number
    issueDate
    expiryDate
  }
}
```

### Ответ (пример)
```graphql
{
  "data": {
    "clientDocuments": [
      {
        "id": "doc-1",
        "type": "PASSPORT",
        "number": "1234 567890",
        "issueDate": "2015-01-01",
        "expiryDate": "2030-01-01"
      }
    ]
  }
}
```