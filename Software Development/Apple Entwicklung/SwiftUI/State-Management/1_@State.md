# @State Property Wrapper

**@State-Properties** werden verwendet, um den Zustand einer View zu verwalten und automatisch ihr UI bei Änderungen des States zu aktualisieren.

**@State-Properties** sollten einfache Struct-Typen haben, wie Sting, Int und Arrays.
Sie sind nicht dafür gedacht in andere Unter-Views geteilt zu werden.
Dafür gibt sollten die Property-Wrapper wie **@EnvironmentObject**, **@ObsevedObject** oder **@Bindable**.

**Beispiel für ein @State-Property:**

```swift
struct ContentView: View {
    @State private var tabCounter = 0
    
    var body: some View {
        Button("Tab Counter: \(tabCounter)") {
            self.tabCounter += 1
        }
    }
}
```

## Two Way Binding

Two Way Bindings sind dafür gedacht, States bei bestimmten Aktionen automatisch zu aktualisieren.

Zum Beispiel bei der Eingabe von Text in einem Textfeld. Der Inhalt des Textfelds wird in einem State gespeichert und soll sich jedes mal nach einer Nutzereingabe aktualisieren.

**Beispiel:**

```swift
struct ContentView: View {
    @State private var userInput = ""
    
    var body: some View {
        TextField("Gebe etwas ein", text: $userInput)
    }
}
```

Das **$** vor **userInput** leitet das Two Way Binding ein.
Dabei wird die Eingabe des Textfields gelesen und gleichzeitig in **userInput** aktualisiert.
