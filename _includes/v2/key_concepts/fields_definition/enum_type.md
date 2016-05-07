# ENUM type

One example of custom type is ENUM. You can read more about what ENUMs are [here](https://en.wikipedia.org/wiki/Enumerated_type). You can easily create ENUM types in Astronomy even when JavaScript does not have native support for it. Let's take a look at the basic example of creating ENUM type.

```js
import { Enum } from 'meteor/jagi:astronomy';

const Status = Enum.create({
  name: 'Status',
  identifiers: ['OPENED', 'CLOSED', 'DONE', 'CANCELED']
});
```

Now you can access each identifier in the following way.

```js
Status.OPENED; // 0
```

Each identifier will represent successive numbers starting from 0.

- OPENED = 0
- CLOSED = 1
- DONE = 2
- CANCELED = 3

You may also want to provide non successive values, then you can define ENUM in the following way.

```js
import { Enum } from 'meteor/jagi:astronomy';

const Status = Enum.create({
  name: 'Status',
  identifiers: {
    OPENED: 1,
    CLOSED: 2,
    DONE: 4,
    CANCELED: 8
  }
});
```

If there is lack of value for some identifier, then it will receive successive number. Let's take a look at the example.

```js
import { Enum } from 'meteor/jagi:astronomy';

const Status = Enum.create({
  name: 'Status',
  identifiers: {
    OPENED: 5,
    CLOSED: null,
    DONE: 15,
    CANCELED: undefined
  }
});
```

And values for each identifier will be.

- OPENED = 5
- CLOSED = 6
- DONE = 15
- CANCELED = 16

**Usage**

Now let's take a look at how to use our `Status` type in the class schema.

```js
import { Class } from 'meteor/jagi:astronomy';

const Issue = Class.create({
  name: 'Issue',
  /* ... */
  fields: {
    status: {
      type: Status
    }
  }
});
```

As you can see it's quite straightforward. You can use this type as any other type. Benefit of having such a type is not only easy way of accessing identifiers but also automatic validation. So, Astronomy will not allow storing in the `status` field any value that is not listed in the ENUM's definition.

**Getting identifier**

In some cases you may want to retrieve identifier for a number value that is stored in a field. Let's take a look how to convert number value to identifier.

```js
import { Enum } from 'meteor/jagi:astronomy';

const Status = Enum.create({
  name: 'Status',
  identifiers: ['OPENED', 'CLOSED', 'DONE', 'CANCELED']
});

var statusNumber = 0;

Status.getIdentifier(statusNumber); // "OPENED"
```

As you can see, it will return the `"OPENED"` string. You can also get all identifiers.

```js
Status.getIdentifiers(); // ["OPENED", "CLOSED", "DONE", "CANCELED"]
```

**Non number values**

You can also define non number ENUMs, however it's not recommended.

```js
import { Enum } from 'meteor/jagi:astronomy';

const Status = Enum.create({
  name: 'Status',
  identifiers: {
    OPENED: 0,
    CLOSED: 'asd',
    DONE: true,
    CANCELED: false
  }
});
```