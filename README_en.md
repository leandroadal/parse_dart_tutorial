# Parse Server Integration Tutorial  

[Leia em Português](README.md)

This project utilizes the `parse_server_sdk` library to interact with Parse Server, facilitating communication with the Parse API. The main purpose of the code is to perform CRUD operations (Create, Read, Update, Delete) on Parse objects.  

---

## Dependencies  

1. **dotenv**: Used to load environment variables from a `.env` file.  
2. **parse_server_sdk**: Library responsible for interacting with the Parse Server.  

### Installing Dependencies  

```bash  
dart pub get  
```

## Parse Server Configuration

The `initializeParse` function initializes the Parse SDK, configuring the `appId`, `clientKey`, and the `Parse API address`. These credentials are loaded from the `.env` file using the `dotenv` package:

```dart
Future<Parse> initializeParse(env) async {  
  String appId = env['PARSE_APP_ID_XLO'] ?? 'defaultAppId';  
  String clientKey = env['PARSE_CLIENT_KEY_XLO'] ?? 'defaultClientKey';  

  return await Parse().initialize(  
    appId,  
    'https://parseapi.back4app.com/',  
    clientKey: clientKey,  
    autoSendSessionId: true,  
    debug: true,  
  );  
}  
```

## Parse Functions

Create Data

Creates a new record in Parse.

```dart
Future<ParseResponse> createData({  
  required String table,  
  required String column,  
  required dynamic data,  
  required String column1,  
  required dynamic data1,  
}) async {  
  final category = ParseObject(table)  
    ..set(column, data)  
    ..set(column1, data1);  

  final response = await category.save();  
  print(response.success);  
  return response;  
}  
```

Example usage:

```dart
await createData(  
  table: 'Categories',  
  column: 'Title',  
  data: 'Sweaters',  
  column1: 'Position',  
  data1: 3,  
);  
```

## Update Data

Updates an existing record in Parse.

```dart
Future<ParseResponse> updateData() async {  
  final category = ParseObject('Categories')  
    ..objectId = 'BJSgu61clb'  
    ..set<int>('Position', 3);  

  final response = await category.save();  
  print(response.success);  
  return response;  
}  
```

## Delete Data

Deletes a specific record from Parse.

```dart
Future<ParseResponse> deleteData() async {  
  final category = ParseObject('Categories')..objectId = 'BJSgu61clb';  

  final response = await category.delete();  
  print(response.success);  
  return response;  
}  
```

## Get Data by ID

Retrieves a specific record by objectId.

```dart
Future<void> getDataById() async {  
  final response = await ParseObject('Categories').getObject('oBranFpS8M');  
  if (response.success) {  
    print(response.result);  
  }  
}  
```

## Get All Data

Retrieves all records from the Categories table.

```dart
Future<void> getAllData() async {  
  final response = await ParseObject('Categories').getAll();  
  if (response.success) {  
    for (var element in response.result) {  
      print(element);  
    }  
  }  
}  
```

## Query Filters

Here are examples of query filters:

### equalsTo (Query Builder)

Filters records where the Position field has the value 2.

```dart
Future<ParseResponse> equalsTo(QueryBuilder<ParseObject> query) async {  
  query.whereEqualTo('Position', 2);  

  final response = await query.query();  
  if (response.success) {  
    print(response.result);  
  }  
  return response;  
}  
```

### whereContains (Query Builder)

Filters records where the Title field contains the substring "Sweaters".

```dart
Future<void> whereContains(QueryBuilder<ParseObject> query) async {  
  query.whereContains('Title', 'Sweaters');  

  final response = await query.query();  
  if (response.success) {  
    print(response.result);  
  }  
}  
```

## How to Run the Code

Configure the .env file with the `PARSE_APP_ID_XLO` and `PARSE_CLIENT_KEY_XLO` variables.

Run `main.dart` to execute CRUD operations and queries.

```bash
dart main.dart  
```
