# MongoDB varios - Note 2026 09 21 07 15 27

```javascript
// * buscar un array que tenga mas de 1 elemento.
{$expr:{$gt:[{$size:'$receptoresNames'},1]}}
```

 