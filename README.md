Create a Flutter application as named "Product List" that consists of the following properties:
•       A stateful widget called ProductList that displays a list of products. 
•       Each product should have a name, price, and a "Buy Now" button.
•       A counter for each product that increments by 1 every time the "Buy Now" button is pressed.
•       When the counter for a product reaches 5, show a dialog box with the message "Congratulations! You've bought 5 {Product Name}!" Replace {Product Name} with the actual name of the product.
•       Use of ListView widget to display the product list.
•       Implementation of navigation to a new page called CartPage when the user presses a "Go to Cart" button. 
•       The CartPage should display the total number of products the user has bought.

![M71](https://github.com/IftikharSikder/Module-7-Assignment/assets/101981180/8e4faf9a-d5da-48f4-8889-b1177d9c77d7)
![M72](https://github.com/IftikharSikder/Module-7-Assignment/assets/101981180/80267d1a-45b8-4a96-93ab-f28b98836449)
![M73](https://github.com/IftikharSikder/Module-7-Assignment/assets/101981180/1c0b1712-6d72-485d-8f6f-b18beef24908)

Code:----------------------------------------
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Product List App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: ProductList(),
    );
  }
}

class ProductList extends StatefulWidget {
  @override
  _ProductListState createState() => _ProductListState();
}

class _ProductListState extends State<ProductList> {
  List<Product> products = [
    Product('Milk', 2.99),
    Product('Bread', 1.99),
    Product('Eggs', 2.49),
    Product('Chicken', 6.99),
    Product('Tomatoes', 0.99),
    Product('Potatoes', 1.49),
    Product('Apples', 1.99),
    Product('Rice', 3.99),
    Product('Pasta', 1.49),
    Product('Yogurt', 2.99),
    Product('Orange Juice', 3.49),
    Product('Cereal', 4.99),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Product List'),
        centerTitle: true,
      ),
      body: Stack(
        children: [
          ListView.builder(
            itemCount: products.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(products[index].name),
                subtitle: Text('\$${products[index].price.toStringAsFixed(2)}'),
                trailing: ProductCounter(
                  product: products[index],
                  onBuyPressed: (product) {
                    setState(() {
                      product.incrementCounter();
                      if (product.counter == 5) {
                        showDialog(
                          context: context,
                          builder: (context) {
                            return AlertDialog(
                              title: Text('Congratulations!'),
                              content: Text('You\'ve bought 5 ${product.name}!'),
                              actions: [
                                TextButton(
                                  child: Text('OK'),
                                  onPressed: () {
                                    Navigator.of(context).pop();
                                  },
                                ),
                              ],
                            );
                          },
                        );
                      }
                    });
                  },
                ),
              );
            },
          ),
          Positioned(
            bottom: 16.0,
            right: 16.0,
            child: FloatingActionButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => CartPage(products: products)),
                );
              },
              child: Icon(Icons.shopping_cart),
            ),
          ),
        ],
      ),
    );
  }
}

class Product {
  String name;
  double price;
  int counter;

  Product(this.name, this.price, {this.counter = 0});

  void incrementCounter() {
    counter++;
  }
}

class ProductCounter extends StatelessWidget {
  final Product product;
  final Function(Product) onBuyPressed;

  ProductCounter({required this.product, required this.onBuyPressed});

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisSize: MainAxisSize.min,
      children: [
        Text('${product.counter}'),
        SizedBox(width: 10),
        ElevatedButton(
          child: Text('Buy Now'),
          onPressed: () {
            onBuyPressed(product);
          },
        ),
      ],
    );
  }
}

class CartPage extends StatelessWidget {
  final List<Product> products;

  CartPage({required this.products});

  int getTotalProducts() {
    int total = 0;
    for (var product in products) {
      total += product.counter;
    }
    return total;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Cart'),
      ),
      body: Center(
        child: Text(
          'Total Products: ${getTotalProducts()}',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}


