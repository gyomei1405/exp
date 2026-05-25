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
    // Note: 10.0.2.2 points directly to your PC's localhost from an Android Emulator.
    var url = Uri.parse("http://10.0.2.2/flutter_api/insert.php");
    
    try {
      var response = await http.post(url, body: {
        "name": nameController.text,
        "email": emailController.text,
        "mobile": mobileController.text,
      });

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
          SnackBar(content: Text("Server Answered: ${response.body}")),
        );
      }
    } catch (e) {
      if (!mounted) return;
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text("Connection Failed: $e")),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("MySQL Insert")),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          children: [
            TextField(
              controller: nameController,
              decoration: const InputDecoration(labelText: "Name"),
            ),
            TextField(
              controller: emailController,
              decoration: const InputDecoration(labelText: "Email"),
            ),
            TextField(
              controller: mobileController,
              decoration: const InputDecoration(labelText: "Mobile"),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: insertData,
              child: const Text("Insert Data"),
            ),
          ],
        ),
      ),
    );
  }
}
