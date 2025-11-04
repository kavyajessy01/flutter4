import 'package:flutter/material.dart';

void main() {
  runApp(MovieTicketBookingApp());
}

class MovieTicketBookingApp extends StatelessWidget {
  const MovieTicketBookingApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Movie Ticket Booking',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MovieBookingScreen(),
    );
  }
}

class MovieBookingScreen extends StatefulWidget {
  const MovieBookingScreen({super.key});

  @override
  _MovieBookingScreenState createState() => _MovieBookingScreenState();
}

class _MovieBookingScreenState extends State<MovieBookingScreen> {
  final TextEditingController _ticketCountController = TextEditingController();
  final TextEditingController _ticketPriceController = TextEditingController();
  final TextEditingController _snackController = TextEditingController();
  final TextEditingController _balanceController = TextEditingController();

  double _totalCost = 0.0;
  String _message = '';
  Color _messageColor = Colors.black;

  void _calculateTotal() {
    setState(() {
      double tickets = double.tryParse(_ticketCountController.text) ?? 0;
      double pricePerTicket = double.tryParse(_ticketPriceController.text) ?? 0;
      double snacks = double.tryParse(_snackController.text) ?? 0;
      double balance = double.tryParse(_balanceController.text) ?? 0;

      if (tickets <= 0 || pricePerTicket <= 0) {
        _message = 'Please enter valid ticket details.';
        _messageColor = Colors.red;
        return;
      }

      _totalCost = (tickets * pricePerTicket) + snacks;

      if (balance < _totalCost) {
        _message =
            'Not enough balance! You need ₹${(_totalCost - balance).toStringAsFixed(2)} more.';
        _messageColor = Colors.red;
      } else {
        _message = 'Booking Successful! Remaining balance: ₹${(balance - _totalCost).toStringAsFixed(2)}';
        _messageColor = Colors.green;
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Movie Ticket Booking'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _ticketCountController,
              decoration: InputDecoration(labelText: 'Number of Tickets'),
              keyboardType: TextInputType.number,
            ),
            TextField(
              controller: _ticketPriceController,
              decoration: InputDecoration(labelText: 'Price per Ticket'),
              keyboardType: TextInputType.number,
            ),
            TextField(
              controller: _snackController,
              decoration: InputDecoration(labelText: 'Snacks/Other Charges'),
              keyboardType: TextInputType.number,
            ),
            TextField(
              controller: _balanceController,
              decoration: InputDecoration(labelText: 'Your Available Balance'),
              keyboardType: TextInputType.number,
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: _calculateTotal,
              child: Text('Book Tickets'),
            ),
            SizedBox(height: 10),
            Text(
              _message,
              style: TextStyle(
                color: _messageColor,
                fontSize: 16,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
