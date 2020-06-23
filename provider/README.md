## Examples managing app state with provider

### COUNTER
Is the starter Flutter application build using provider
-  Uses ChangeNotifierProvider because that's a simple way to rebuild widgets when a model changes. We could also just use Provider, 
but then we would have to listen to Counter ourselves.
 
```dart
runApp(
    ChangeNotifierProvider(
      // Initialize the model in the builder. That way, Provider
      // can own Counter's lifecycle, making sure to call `dispose`
      // when not needed anymore.
      create : (context) => Counter(),
      child: MyApp(),
    )
  );
```
- the simplest possible model, with just one field.

```dart
/// [ChangeNotifier] is a class in `flutter:foundation`. 
class Counter with ChangeNotifier {
  int value = 0;

  void increment(){
    value += 1;
    notifyListeners();
  }
}
```
-  the Consumer which looks for an ancestor Provider widget and retrieves its model (Counter, in this case).  Then it uses that model to build widgets, and will trigger rebuilds if the model is updated.
```dart
Consumer<Counter>(
    builder: (context, counter, child) => Text(
    '${counter.value}',
    style: Theme.of(context).textTheme.headline4,
    ),
),
```
-  Provider.of is another way to access the model object held by an ancestor Provider. By default, even this listens to changes in the model, and rebuilds the whole encompassing widget when notified. By using `listen: false` below, we are disabling that behavior.
```dart
floatingActionButton: FloatingActionButton(
    onPressed: () =>
        Provider.of<Counter>(context, listen: false).increment(),
    tooltip: 'Increment',
    child: Icon(Icons.add),
),
```

### SHOPPER