import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';

main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  MyAppState createState() => MyAppState();
}

class MyAppState extends State<MyApp> {
  var msgController = TextEditingController();
  late String chatmsg;

  @override
  Widget build(BuildContext context) {
    // Firebase.initializeApp();

    var base = FirebaseFirestore.instance;
    // var deviceWidth = MediaQuery.of(context).size.width;

    /* myget() async {
      var d = await base.collection("students").get();
      List fav = [];
      for (var i in d.docs) {
        print(i.data());
        fav.add(i.data());
        print(fav);
        Text("($fav)");
      }
    }*/

    return MaterialApp(
        debugShowCheckedModeBanner: false,
        theme: ThemeData(
          primarySwatch: Colors.amber,
        ),
        home: Scaffold(
            appBar: AppBar(
              title: Text("Flutter App"),
            ),
            body: Center(
              child: Container(
                  child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                crossAxisAlignment: CrossAxisAlignment.center,
                children: [
                  StreamBuilder<QuerySnapshot>(
                    builder: (context, snaphot) {
                      print("new data come");
                      //var y = this.fav.docs[0].data();
                      //print(y);
                      //var msg = base.collection("students").get();
                      var msg = snaphot.data!.docs;
                      //print(msg[0].data());

                      List<Widget> y = [];
                      for (var i in msg) {
                        var msgText = i.data();
                        var msgWidget = Text('$msgText');
                        y.add(msgWidget);
                      }

                      return Container(
                        child: Column(
                          children: y,
                        ),
                      );
                    },
                    stream: base.collection("database").snapshots(),
                  ),
                  SizedBox(
                    height: 10,
                  ),
                  Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Container(
                        //width: deviceWidth * 0.60,
                        width: 300,
                        child: TextField(
                          //obscureText: true,
                          controller: msgController,
                          decoration: InputDecoration(
                              hintText: "Enter your data...",
                              border: OutlineInputBorder(
                                  borderRadius: BorderRadius.circular(20))),
                          onChanged: (value) {
                            chatmsg = value;
                          },
                        ),
                      ),
                      ElevatedButton(
                          onPressed: () {
                            msgController.clear();
                            base.collection("database").add({"text": chatmsg});
                            //  .add({"name": "Max", "email": "m@lw.com"});
                            print("working");
                            print(chatmsg);
                          },
                          child: Text("Send")),
                      SizedBox(
                        height: 20,
                      ),
                      /*Center(
                        child: ElevatedButton(
                          child: Text("recieve"),
                          onPressed: () {
                            myget();
                            print("Data recieved");
                          },
                        ),
                      ),*/
                    ],
                  ),
                ],
              )),
            )));
  }
}
  
  
  
