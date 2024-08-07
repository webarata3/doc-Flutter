# ネットワーク

## ネットワークを使ったアプリ

スマートフォンは、Wi-Fiやモバイルネットワークに接続され、いつでもインターネットから情報を取得することができます。アプリでも当然ネットワーク通信をすることができます。

``` json linenums="1"
[
  {
    "publishingOffice": "金沢地方気象台",
    "reportDatetime": "2024-08-07T11:00:00+09:00",
    "timeSeries": [
      {
        "timeDefines": [
          "2024-08-07T11:00:00+09:00",
          "2024-08-08T00:00:00+09:00",
          "2024-08-09T00:00:00+09:00"
        ],
        "areas": [
          {
            "area": {
              "name": "加賀",
              "code": "170010"
            },
            "weatherCodes": [
              "201",
              "101",
              "201"
            ],
            "weathers": [
              "くもり　時々　晴れ　所により　昼過ぎ　から　夕方　雨　で　雷を伴い　激しく　降る",
              "晴れ　時々　くもり　所により　昼過ぎ　から　夕方　雨　で　雷を伴う",
              "くもり　時々　晴れ"
            ],
            "winds": [
              "北の風　後　東の風",
              "東の風　日中　北の風",
              "東の風　後　北の風"
            ],
            "waves": [
              "０．５メートル",
              "０．５メートル",
              "０．５メートル　後　１メートル"
            ]
          },
          {
            "area": {
              "name": "能登",
              "code": "170020"
            },
            "weatherCodes": [
              "201",
              "101",
              "201"
            ],
            "weathers": [
              "くもり　時々　晴れ　所により　昼過ぎ　から　夜のはじめ頃　雨",
              "晴れ　時々　くもり",
              "くもり　時々　晴れ"
            ],
            "winds": [
              "北の風",
              "北の風",
              "南の風　後　北の風"
            ],
            "waves": [
              "０．５メートル",
              "０．５メートル",
              "０．５メートル　後　１メートル"
            ]
          }
        ]
      },
      {
        "timeDefines": [
          "2024-08-07T12:00:00+09:00",
          "2024-08-07T18:00:00+09:00",
          "2024-08-08T00:00:00+09:00",
          "2024-08-08T06:00:00+09:00",
          "2024-08-08T12:00:00+09:00",
          "2024-08-08T18:00:00+09:00"
        ],
        "areas": [
          {
            "area": {
              "name": "加賀",
              "code": "170010"
            },
            "pops": [
              "30",
              "20",
              "20",
              "10",
              "20",
              "10"
            ]
          },
          {
            "area": {
              "name": "能登",
              "code": "170020"
            },
            "pops": [
              "20",
              "20",
              "10",
              "0",
              "10",
              "10"
            ]
          }
        ]
      },
      {
        "timeDefines": [
          "2024-08-07T09:00:00+09:00",
          "2024-08-07T00:00:00+09:00",
          "2024-08-08T00:00:00+09:00",
          "2024-08-08T09:00:00+09:00"
        ],
        "areas": [
          {
            "area": {
              "name": "金沢",
              "code": "56227"
            },
            "temps": [
              "31",
              "31",
              "26",
              "32"
            ]
          },
          {
            "area": {
              "name": "輪島",
              "code": "56052"
            },
            "temps": [
              "30",
              "30",
              "24",
              "31"
            ]
          }
        ]
      }
    ]
  },
  {
    "publishingOffice": "金沢地方気象台",
    "reportDatetime": "2024-08-07T11:00:00+09:00",
    "timeSeries": [
      {
        "timeDefines": [
          "2024-08-08T00:00:00+09:00",
          "2024-08-09T00:00:00+09:00",
          "2024-08-10T00:00:00+09:00",
          "2024-08-11T00:00:00+09:00",
          "2024-08-12T00:00:00+09:00",
          "2024-08-13T00:00:00+09:00",
          "2024-08-14T00:00:00+09:00"
        ],
        "areas": [
          {
            "area": {
              "name": "石川県",
              "code": "170000"
            },
            "weatherCodes": [
              "101",
              "201",
              "201",
              "101",
              "101",
              "101",
              "201"
            ],
            "pops": [
              "",
              "20",
              "30",
              "20",
              "20",
              "20",
              "30"
            ],
            "reliabilities": [
              "",
              "",
              "A",
              "A",
              "A",
              "B",
              "B"
            ]
          }
        ]
      },
      {
        "timeDefines": [
          "2024-08-08T00:00:00+09:00",
          "2024-08-09T00:00:00+09:00",
          "2024-08-10T00:00:00+09:00",
          "2024-08-11T00:00:00+09:00",
          "2024-08-12T00:00:00+09:00",
          "2024-08-13T00:00:00+09:00",
          "2024-08-14T00:00:00+09:00"
        ],
        "areas": [
          {
            "area": {
              "name": "金沢",
              "code": "56227"
            },
            "tempsMin": [
              "",
              "26",
              "26",
              "26",
              "26",
              "26",
              "26"
            ],
            "tempsMinUpper": [
              "",
              "28",
              "28",
              "28",
              "29",
              "29",
              "29"
            ],
            "tempsMinLower": [
              "",
              "25",
              "24",
              "24",
              "24",
              "24",
              "24"
            ],
            "tempsMax": [
              "",
              "32",
              "33",
              "34",
              "35",
              "35",
              "34"
            ],
            "tempsMaxUpper": [
              "",
              "34",
              "34",
              "36",
              "38",
              "38",
              "36"
            ],
            "tempsMaxLower": [
              "",
              "31",
              "31",
              "32",
              "33",
              "32",
              "32"
            ]
          }
        ]
      }
    ],
    "tempAverage": {
      "areas": [
        {
          "area": {
            "name": "金沢",
            "code": "56227"
          },
          "min": "24.6",
          "max": "31.6"
        }
      ]
    },
    "precipAverage": {
      "areas": [
        {
          "area": {
            "name": "金沢",
            "code": "56227"
          },
          "min": "9.0",
          "max": "40.7"
        }
      ]
    }
  }
]
```

``` yaml linenums="1" title="pubspec.yaml"
dependencies:
  flutter:
    sdk: flutter
  http: ^1.2.2
```

``` dart linenums="1" title="main.dart"
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  var _message = 'コードを入力して検索を押してください';
  final _locTextController = TextEditingController();
  var _weather = '';

  @override
  void initState() {
    super.initState();
    // 石川県を初期表示
    _locTextController.text = '170000';
  }

  void _error(String message) {
    setState(() {
      _message = message;
    });
  }

  void _fetch() {
    final code = _locTextController.text;
    if (code.isEmpty) {
      _error('地域コードを入力してください');
      return;
    }

    final uri = Uri.parse(
        'https://www.jma.go.jp/bosai/forecast/data/forecast/$code.json');
    http.get(uri).then((response) {
      if (response.statusCode != 200) {
        _error('URLが違います');
        return;
      }
      final body = utf8.decode(response.bodyBytes);
      final json = jsonDecode(body);
      setState(() {
        _weather = json[0]['timeSeries'][0]['areas'][0]['weathers'][0];
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Column(
        children: [
          Text(_message),
          Padding(
            padding: const EdgeInsets.all(10.0),
            child: TextField(
              decoration: const InputDecoration(
                border: OutlineInputBorder(),
                labelText: '地域コード',
              ),
              controller: _locTextController,
            ),
          ),
          FilledButton(
            onPressed: _fetch,
            child: const Text('検索'),
          ),
          Text(_weather),
        ],
      ),
    );
  }
}
```
