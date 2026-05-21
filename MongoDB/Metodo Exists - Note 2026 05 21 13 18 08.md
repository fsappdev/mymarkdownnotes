# Metodo Exists - Note 2026 05 21 13 18 08

 

```javascript
const mongoose = require( 'mongoose ' ); const Schema = mongoose. Schema;

const UserSchema = new Schema ({ name: String, email: String, phone: String });

const User = mongoose. model ( 'User', UserSchema) ;

User. find({ phone: { $exists: true } }, function(err, users) { if (err) { console. log(err); } else { console. log(users) ; } });
```

 