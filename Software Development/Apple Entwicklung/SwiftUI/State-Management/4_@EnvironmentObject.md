# @EnvironmentObject Property Wrapper

Sollen Daten mehreren Views auf verschiedenen Ebenen bekannt sein, wird **@EnvironmentObject** eingesetzt. Diese Daten werden **shared data** genannt.

## Beispiel

Shared Data müssen (ab iOS 17) mit @Observable sein oder (vor iOS 17) das **ObservableObject** Protocol implementieren. In diesem Beispiel wird auf die (meiner Meinung nach bessere) Variante ab iOS 17 eingegangen. Für mehr Infors siehe [Ofizielle Doku](https://developer.apple.com/documentation/swiftui/migrating-from-the-observable-object-protocol-to-the-observable-macro)

```swift
import SwiftUI
import Observation

@Observable class Library {
  var books [Book] = [Book(), Book(), Book()];
}
```

Jede View hält einen **.environment** Modifier bereit, in dem die Shared Data übergeben werden können. Hierdurch werden die Shared Data einer View bekannt gemacht.

**Shared Data in einer Root-View bekannt machen:**

```swift
@main
struct BookReaderApp: App {
    @State private var library = Library()

    var body: some Scene {
        WindowGroup {
            LibraryView()
                .environment(library)
        }
    }
}
```

**Shared Data in einer View aufrufen:**
```swift
struct LibraryView: View {
    @Environment(Library.self) private var library
    
    var body: some View {
        List(library.books) { book in
            BookView(book: book)
        }
    }
}
```
