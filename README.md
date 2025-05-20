## Hi there 👋

<!--
**kamil12-us/kamil12-us** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/ecommerce");

// Models
const Product = mongoose.model("Product", {
  name: String,
  price: Number,
  image: String,
});

const User = mongoose.model("User", {
  email: String,
  password: String,
});

// Routes
app.get("/api/products", async (req, res) => {
  const products = await Product.find();
  res.json(products);
});

app.post("/api/register", async (req, res) => {
  const user = new User(req.body);
  await user.save();
  res.status(201).json(user);
});

app.post("/api/login", async (req, res) => {
  const user = await User.findOne(req.body);
  if (user) res.json({ success: true, user });
  else res.status(401).json({ success: false });
});

app.listen(5000, () => console.log("Server berjalan di http://localhost:5000"));
