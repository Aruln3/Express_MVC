### Express_MVC

Model-View-Controller (MVC) architecture - The MVC pattern helps in organizing your application into three main components: Models, Views, and Controllers, making it easier to manage and scale.

New directory for project and initialization!
`mkdir express-mvc-app`
`cd express-mvc-app`
`npm init -y`
`npm install express ejs body-parser`

###Project Structure
express-mvc-app/
├── controllers/
│   └── userController.js
├── models/
│   └── userModel.js
├── views/
│   └── index.ejs
├── routes/
│   └── userRoutes.js
├── app.js
└── package.json

##Model
The models folder (userModel.js) will represent the data structure and business logic.

```JavaScript
// models/userModel.js
const users = [];

class User {
  constructor(id, name) {
    this.id = id;
    this.name = name;
  }

  static findAll() {
    return users;
  }

  static findById(id) {
    return users.find(user => user.id === id);
  }

  static create(user) {
    users.push(user);
  }
}

module.exports = User;
```

##Controller
The controllers folder (userController.js) will handle the application logic and interact with the model.

```JavaScript
// controllers/userController.js
const User = require('../models/userModel');

exports.getAllUsers = (req, res) => {
  const users = User.findAll();
  res.render('index', { users });
};

exports.createUser = (req, res) => {
  const { id, name } = req.body;
  const user = new User(id, name);
  User.create(user);
  res.redirect('/');
};
```

##Routes
The routes folder (userRoutes.js) will define the application routes.

```JavaScript
// routes/userRoutes.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

router.get('/', userController.getAllUsers);
router.post('/create', userController.createUser);

module.exports = router;
```

##Views
The views folder (index.ejs) This file will define the HTML template.

```HTML
<!-- views/index.ejs -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>User List</title>
</head>
<body>
  <h1>User List</h1>
  <ul>
    <% users.forEach(user => { %>
      <li><%= user.name %></li>
    <% }) %>
  </ul>
  <form action="/create" method="POST">
    <input type="text" name="id" placeholder="ID" required>
    <input type="text" name="name" placeholder="Name" required>
    <button type="submit">Add User</button>
  </form>
</body>
</html>
```

##Main Application
Finally, The Main application file app.js.

```JavaScript
// app.js
const express = require('express');
const bodyParser = require('body-parser');
const userRoutes = require('./routes/userRoutes');

const app = express();
const port = 3000;

// Set up the view engine
app.set('view engine', 'ejs');

// Middleware to parse request body
app.use(bodyParser.urlencoded({ extended: false }));

// Use the user routes
app.use('/', userRoutes);

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```
###Summary
This setup demonstrates a basic MVC architecture using Node.js and Express.js. The Model handles data and business logic, the View renders the user interface, and the Controller processes incoming requests and returns responses. This structure helps in keeping the code organized and maintainable.
