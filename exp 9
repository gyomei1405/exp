MySQL -> 
3306 -> 3307

apache 
listen 80 -> 8080
localhost:8080

C:\xampp\htdocs\ 
create a folder called flutter_api 
and text file name called insert.php


phpmyadmin->
http://localhost:8080/phpmyadmin/index.php

CREATE DATABASE flutter_db;
USE flutter_db;

CREATE TABLE users (
 id INT AUTO_INCREMENT PRIMARY KEY,
 name VARCHAR(100),
 email VARCHAR(100),
 mobile VARCHAR(15)
);


insert.php->

<?php
// Connected to your database engine via custom port 3307
$conn = new mysqli("localhost", "root", "", "flutter_db", 3307);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

// Intercepting data sent from the mobile app interface
$name = $_POST['name'] ?? '';
$email = $_POST['email'] ?? '';
$mobile = $_POST['mobile'] ?? '';

if (!empty($name) && !empty($email) && !empty($mobile)) {
    $sql = "INSERT INTO users (name, email, mobile) VALUES ('$name', '$email', '$mobile')";
    if ($conn->query($sql) === TRUE) {
        echo "Success"; 
    } else {
        echo "Error";
    }
} else {
    echo "Empty Fields";
}
$conn->close();
?>





dependencies:
  flutter:
    sdk: flutter
  http: ^0.13.6




import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: InsertPage(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class InsertPage extends StatefulWidget {
  const InsertPage({super.key});

  @override
  _InsertPageState createState() => _InsertPageState();
}

class _InsertPageState extends State<InsertPage> {
  final TextEditingController nameController = TextEditingController();
  final TextEditingController emailController = TextEditingController();
  final TextEditingController mobileController = TextEditingController();

  Future<void> insertData() async {
    // 10.0.2.2 routes your Android emulator to your computer's server engine.
    // Port 8080 points directly to your active Apache instance.
    var url = Uri.parse("http://10.0.2.2:8080/flutter_api/insert.php");

    try {
      var response = await http.post(
        url,
        body: {
          "name": nameController.text,
          "email": emailController.text,
          "mobile": mobileController.text,
        },
      );

      if (!mounted) return;

      if (response.body.trim() == "Success") {
        ScaffoldMessenger.of(context).showSnackBar(
          const SnackBar(content: Text("Data Inserted Successfully")),
        );
        nameController.clear();
        emailController.clear();
        mobileController.clear();
      } else {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text("Server Side Error: ${response.body}")),
        );
      }
    } catch (e) {
      if (!mounted) return;
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text("Network Connection Encountered an Issue: $e")),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("MySQL Data Insertion Lab")),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          children: [
            TextField(
              controller: nameController,
              decoration: const InputDecoration(labelText: "Enter Full Name"),
            ),
            TextField(
              controller: emailController,
              decoration: const InputDecoration(
                labelText: "Enter Email Address",
              ),
            ),
            TextField(
              controller: mobileController,
              decoration: const InputDecoration(
                labelText: "Enter Mobile Number",
              ),
            ),
            const SizedBox(height: 25),
            ElevatedButton(
              onPressed: insertData,
              child: const Text("Send Data to MySQL"),
            ),
          ],
        ),
      ),
    );
  }
}
