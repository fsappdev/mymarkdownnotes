# Buscar por fechas en mongo - Note 2026 06 23 07 33 33

1 documentos creados en actuaciones 2024

```javascript
{
  updatedAt: {
    $gte: ISODate("2024-02-01T00:00:00.000Z"),
    $lt: ISODate("2024-03-01T00:00:00.000Z")
  }
}
```

```
{"nombre": {"$regex": "Toledo$", "$options": "i"}}
```

 