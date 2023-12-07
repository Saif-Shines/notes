# Intro to MongoDB

# Installation and Setup

Use the following repository

```bash
> git clone https://github.com/FrontendMasters/intro-mongo-db
> brew install mongodb-atlas
> mongod # creates default databases
> mongo # gives me a shell to interact with the data base

~/MyLab/intro-mongo-db master
❯ mongo
MongoDB shell version v4.2.2
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("429391db-a38a-463e-aa50-c9190dfdd10a") }
MongoDB server version: 4.4.3
WARNING: shell and server versions do not match
Server has startup warnings: 
{"t":{"$date":"2023-05-30T20:07:10.474+05:30"},"s":"W",  "c":"CONTROL",  "id":22120,   "ctx":"initandlisten","msg":"Access control is not enabled for the database. Read and write access to data and configuration is unrestricted","tags":["startupWarnings"]}
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---

> show dbs
admin         0.000GB
adoption      0.000GB
config        0.000GB
local         0.000GB
passport-jwt  0.000GB
> use todos
switched to db todos
> show collections
> db.help()
```

# MongoDB Q&A

- Mongo is mostly written in C++, then Javascript and a little bit of Python and Go.
- When to use MongoDB
    - Especially when you are getting started with something
    - When shapes are changing constantly
    - You don’t want to run migrations every time you change shapes.
    - Especially with JS and Node, Mongo chimes along, taking advantage of event-driven architecture.
    - Perfect of E-Commerce Applications.
- Compare it with other SQL databases
    - It depends on the kind of app you’re building. For example, if you have 100% clarity over primary keys, structure, and everything, and if you are like Google of the world, then yes maybe Postgres and SQL DBs are good because they offer completeness.
    - For example, I wouldn’t use it in cases like social network apps. It’s more of a Graph use-case or time-series scenario to run like analytics.
    
    # Mongoose
    
    ```jsx
    const mongoose = require("mongoose");
    
    const connect = () => {
      return mongoose.connect("mongodb://localhost:27017/whatver");
    };
    
    const student = new mongoose.Schema({
      firstName: String,
    });
    
    const Student = mongoose.model("student", student);
    
    connect()
      .then(async function connection() {
        const student = await Student.create({ firstName: "Saif" });
        console.log(student);
      })
      .catch((e) => console.error(e));
    ```
    
    1. Install the driver `mongoose`
    2. Tell Mongoose to connect to the MongoDB database running locally using `mongoose.connect(..)`. This returns a promise. 
    3. Create a Schema and Associate it with a Model. In this case case `new mongoose.Schema(..)` is associated with `mongooes.model(..)` by passing return value of model.

<aside>
💡 `.exec()` command is to say your chain is complete, and you will not add more methods as filters.

</aside>

# Associations

How do you have one document refer to another, for example, in a different collection? 

```jsx
studentSchema = new mongoose.Schema(
  {
    firstName: {
      type: String,
      required: true,
      unique: true,
    },
    faveFoods: [{ String }],
    info: {
      school: {
        type: String,
      },
      shoeSize: {
        type: Number,
      },
    },
    school: {
      type: mongoose.Schema.Types.ObjectId,
      required: true,
      ref: "school",
    },
  },
  { timestamps: true }
);
```

1. Put one document into another like the following Schema (Yellow Highlight)
2. Refer to a collection name by declaring that one of the schema's fields is an ID of the document. (Blue Highlight)

# Node App Example - Simple

```jsx
const mongoose = require("mongoose");
const express = require("express");
const app = express();
const morgan = require("dev");
const { urlencoded, json } = require("body-parser");
const noteSchema = new mongoose.Schema({
  title: {
    type: String,
    required: true,
    unique: true,
  },
  body: {
    type: String,
    minlength: 10,
  },
});

const Note = mongoose.model("note", noteSchema);

app.use(morgan("dev"));
app.use(urlencoded({ extended: true }));
app.use(json());

app.get("/note", async function (req, res) {
  const notes = await Note.find({}).lean().exec(); // `lean` removes unnecessary properties
  res.status(200).json(notes);
});

app.post("/note", async function (req, res) {
  const noteToBeCreated = req.body;
  const note = await Note.create(noteToBeCreated);
  res.status(201).json(note);
});

async function connect() {
  await mongoose.connect("mongodb://localhost:27017/whatver");
}

const School = mongoose.model("school", school);

connect()
  .then(async (connection) => {
    app.listen(5000);
  })
  .catch(console.error);
```

### Querying associations with populate

```jsx
student = new mongoose.Schema(
  {
    firstName: {
      type: String,
      required: true,
      unique: true,
    },
    faveFoods: [{ String }],
    info: {
      school: {
        type: String,
      },
      shoeSize: {
        type: Number,
      },
    },
    school: {
      type: mongoose.Schema.Types.ObjectId,
      required: true,
      ref: "school",
    },
  },
  { timestamps: true }
);

const school = new mongoose.Schema({
  name: String,
});

const School = mongoose.model("school", school);
Student = mongoose.model("student", student);

connect()
  .then(async (connection) => {
    const school = await School.create({ name: "mlk elementary" });
    const student = await Student.create({
      firstName: "Trisha",
      school: school._id,
    });

    const match = await Student.findById(student.id).populate("school").exec();
    console.info(match);
  })
  .catch(console.error);
```

Let’s understand how I query to get the following result:

```bash
# I found a student record, but the response has a school object without it actually
# being in the database.
{
  _id: 647a1223e57fa90d76d66224,
  firstName: 'Trisha',
  school: { _id: 647a1222e57fa90d76d66223, name: 'mlk elementary', __v: 0 },
  faveFoods: [],
  createdAt: 2023-06-02T16:00:35.143Z,
  updatedAt: 2023-06-02T16:00:35.143Z,
  __v: 0
}
```

1. You create two Models - `Student` and `School` . Both have different Schema.
2. The `Student`Model will point (like a foreign key to) `School` model through `ref`
3. During your **********connection********** with DB, I created two records, one for school and another for students. I also stored their IDs of them in the runtime variables.
4. The query happens at `Student.findById(..)`.
    1. You query the Student that you just created. 
    2. Then chain with `.populate()` to have associated ***school*** collection document within the response.

That’s how you query the associations.

<aside>
💡 There’s a huge difference between `_id` and `id`.
The mongoDB adds `_id` property for the records it will store. However, because developers find it harder, mongoose objects add `id` on the fly.

</aside>

### Querying with Filters and Modifiers

Start by creading one few field - `staff`

```jsx
const school = new mongoose.Schema({
  name: String,
  openSince: Number,
  students: Number,
  isGreat: Boolean,
  staff: [{ type: String }], // staff: [String] also works
});

// create model
const School = mongoose.model("school", school);

// create record objects
const schoolConfig = {
      name: "mlk elementary",
      openSince: 2009,
      students: 1000,
      isGreat: true,
      staff: ["a", "b", "c"],
    };
    const school2 = {
      name: "Larry Middle School",
      openSince: 1980,
      students: 600,
      isGreat: false,
      staff: ["v", "b", "g"],
    };
```

Now when your JS connects to MongoDB:

```jsx
// Assume `schoolConfig` and `school2` are two schools.
const schools = await School.create([schoolConfig, school2]);
    const match = await School.findOne({
      students: { $gt: 600, $lt: 800 }, // filter
      isGreat: true,
    }).exec();
    const queryRes = await School.findOne({
      staff: "b",
      $in: { staff: ["v", "g"] },
    })
.sort({ openSince: -1 }) // modifiers
.limit(2)
.exec();
    console.log(queryRes);
```

# Virtuals & Hooks - Middleware

It’s not something that’s saved in the database, it’s computed on the fly. 

```jsx
school.virtual()
```

- A *******getter******* is a function that is invoked when thread of execution tries to get the value.
- A ************setter************ is a function that is invoked when thread of execution tries to set the value.

```jsx
school.pre("save", function (doc, next) {
  console.log("before the document has been saved");
});

school.post("save", function (doc, next) {
  console.log("after the document has been saved");
	next();
});

school.virtual("staffCount").get(function () {
  console.log("in virtual");
  return this.staff.length;
});
```

### Indexes

If you have millions of records in a collection, and what you are looking for is the last one, you really do not want to search through millions records. Indexing a database means to imagine a key value pair, where you collect and remember the keys so that you get value quickly right when you need it.

<aside>
💡 Indexes are usually stored in runtime memory.

</aside>

****Compound Indexes****

When you create Schema and attach `unique:true` to a property. You already created an index. Mongo will “remember the check” in a separate book, as the new record gets created, it looks up on this book and avoid a duplicate. 

Now consider following example

```jsx
const school = new mongoose.Schema({
  district: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "district",
  },
  name: {
    type: String,
    unique: true,
  },
  openSince: Number,
  students: Number,
  isGreat: Boolean,
  staff: [{ type: String }],
});const school = new mongoose.Schema({
  district: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "district",
  },
  name: {
    type: String,
    unique: true,
  },
  openSince: Number,
  students: Number,
  isGreat: Boolean,
  staff: [{ type: String }],
});

school.index(
  {
    district: 1,
    name: 1,
  },
  { unique: true }
);
```

With the above code (within a collection), it’s not possible for you to ever create a school name that’s unique per district. (It’s always per collection). We want name to be unique but scoped to a district.

<aside>
💡 The order of `district:1` and `name:1` is important. It means, One School ******Name Unique per District. But not One District name unique per school, which doesn’t make sense.******

</aside>