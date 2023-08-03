# todo_app

Basic DayToDay ToDo App

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.




import 'package:flutter/material.dart';
import '../colors.dart';

class Home extends StatefulWidget {
const Home({Key? key}) : super(key: key);

@override
State<Home> createState() => _HomeState();
}

class _HomeState extends State<Home> {
final todosList = ToDo.todoList();
List<ToDo> _foundToDo = [];
final _todoController = TextEditingController();

@override
void initState() {
_foundToDo = todosList;
super.initState();
}

@override
Widget build(BuildContext context) {
return Scaffold(
backgroundColor: BGColor,
appBar: _buildAppBar(),
body: Stack(
children: [
Container(
padding: const EdgeInsets.symmetric(
horizontal: 20,
vertical: 15,
),
child: Column(
children: [
searchBox(),
Expanded(
child: ListView(
children: [
Container(
margin: const EdgeInsets.only(
top: 50,
bottom: 20,
),
child: const Text(
'All Tasks',
style: TextStyle(
fontSize: 30,
fontWeight: FontWeight.w500,
),
),
),
for (ToDo todoo in _foundToDo.reversed)
ToDoItem(
todo: todoo,
onToDoChanged: _handleToDoChange,
onDeleteItem: _deleteToDoItem,
),
],
),
)
],
),
),
Align(
alignment: Alignment.bottomCenter,
child: Row(children: [
Expanded(
child: Container(
margin: const EdgeInsets.only(
bottom: 20,
right: 20,
left: 20,
),
padding: const EdgeInsets.symmetric(
horizontal: 20,
vertical: 5,
),
decoration: BoxDecoration(
color: Colors.white,
boxShadow: const [
BoxShadow(
color: Colors.grey,
offset: Offset(0.0, 0.0),
blurRadius: 10.0,
spreadRadius: 0.0,
),
],
borderRadius: BorderRadius.circular(10),
),
child: TextField(
controller: _todoController,
decoration: const InputDecoration(
hintText: 'Add a new todo item',
border: InputBorder.none),
),
),
),
Container(
margin: const EdgeInsets.only(
bottom: 20,
right: 20,
),
child: ElevatedButton(
onPressed: () {
_addToDoItem(_todoController.text);
},
style: ElevatedButton.styleFrom(
primary: Orange,
minimumSize: Size(50, 50),
elevation: 10,
),
child: const Text(
'+',
style: TextStyle(
fontSize: 30,
),
),
),
),
]),
),
],
),
);
}

void _handleToDoChange(ToDo todo) {
setState(() {
todo.isDone = !todo.isDone;
});
}

void _deleteToDoItem(String id) {
setState(() {
todosList.removeWhere((item) => item.id == id);
});
}

void _addToDoItem(String toDo) {
setState(() {
todosList.add(ToDo(
id: DateTime.now().millisecondsSinceEpoch.toString(),
todoText: toDo,
));
});
_todoController.clear();
}

void _runFilter(String enteredKeyword) {
List<ToDo> results = [];
if (enteredKeyword.isEmpty) {
results = todosList;
} else {
results = todosList
.where((item) => item.todoText!
.toLowerCase()
.contains(enteredKeyword.toLowerCase()))
.toList();
}

    setState(() {
      _foundToDo = results;
    });
}

Widget searchBox() {
return Container(
padding: const EdgeInsets.symmetric(horizontal: 15),
decoration: BoxDecoration(
color: Colors.white,
borderRadius: BorderRadius.circular(20),
),
child: TextField(
onChanged: (value) => _runFilter(value),
decoration: const InputDecoration(
contentPadding: EdgeInsets.all(0),
prefixIcon: Icon(
Icons.search,
color: Black,
size: 20,
),
prefixIconConstraints: BoxConstraints(
maxHeight: 20,
minWidth: 25,
),
border: InputBorder.none,
hintText: 'Search',
hintStyle: TextStyle(color: Grey),
),
),
);
}

AppBar _buildAppBar() {
return AppBar(
backgroundColor: BGColor,
elevation: 0,
title: Row(mainAxisAlignment: MainAxisAlignment.spaceBetween, children: [
const Icon(
Icons.menu,
color: Black,
size: 30,
),
Container(
height: 30,
width: 30,
child: ClipRRect(
borderRadius: BorderRadius.circular(20),
child: Image.asset('assets/avatar.jpeg'),
),
),
]),
);
}
}





//  ToDo List
class ToDo {
String? id;
String? todoText;
bool isDone;

ToDo({
required this.id,
required this.todoText,
this.isDone = false,
});

static List<ToDo> todoList() {
return [
ToDo(id: '01', todoText: 'Add ToDo List', isDone: true ),
];
}
}



//  ToDo Features
class ToDoItem extends StatelessWidget {
final ToDo todo;
final onToDoChanged;
final onDeleteItem;

const ToDoItem({
Key? key,
required this.todo,
required this.onToDoChanged,
required this.onDeleteItem,
}) : super(key: key);

@override
Widget build(BuildContext context) {
return Container(
margin: const EdgeInsets.only(bottom: 20),
child: ListTile(
onTap: () {
// print('Clicked on Todo Item.');
onToDoChanged(todo);
},
shape: RoundedRectangleBorder(
borderRadius: BorderRadius.circular(20),
),
contentPadding: const EdgeInsets.symmetric(horizontal: 20, vertical: 5),
tileColor: Colors.white,
leading: Icon(
todo.isDone ? Icons.check_box : Icons.check_box_outline_blank,
color: Orange,
),
title: Text(
todo.todoText!,
style: TextStyle(
fontSize: 16,
color: Black,
decoration: todo.isDone ? TextDecoration.lineThrough : null,
),
),
trailing: Container(
padding: const EdgeInsets.all(0),
margin: const EdgeInsets.symmetric(vertical: 12),
height: 35,
width: 35,
decoration: BoxDecoration(
color: Red,
borderRadius: BorderRadius.circular(5),
),
child: IconButton(
color: Colors.white,
iconSize: 18,
icon: const Icon(Icons.delete),
onPressed: () {
// print('Clicked on delete icon');
onDeleteItem(todo.id);
},
),
),
),
);
}
}