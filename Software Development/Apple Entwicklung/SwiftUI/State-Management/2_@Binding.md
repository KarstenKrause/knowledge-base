# @Binding Property Wrapper

Angenommen, eine View ruft eine Kind-View auf.
Diese Kind-View soll **@State**-Properties der Eltern-View bei bestimmten Aktionen ändern. Hierfür wird in der Kind-View ein **@Binding Property Wrapper** benötigt.

Wichtig hierbei ist, dass ein **@Binding** kein Wert zugewiesen werden darf, da er diesen über seine Eltern-View bezieht.

## Beispiel

Der Sheet wird nur dann angezeigt, wenn **showSheet** = true ist.

```swift
struct ParentView: View {
  @State private var showSheet = false

  var body: some View {
    VStack {
      Button("Show Sheet") {
        showSheet = true
      }
    }
    .sheet(isPresented: $showSheet) {
      // @Binding-Property wird im Initializer gesetzt
      // mittels Two-Way-Binding
      ChildView(showSheet: $showSheet)
    }
  }
}
```

Der State **showSheet** der **ParentView()** wird in der **ChildView()** mittels **@Binding** gesetzt.

```swift
struct ChildView: View {
  @Binding var showSheet

  var body: some View {
    Button("Dismiss Sheet") {
      showSheet = false
    }
  }
}
```

Theoretisch könnten **@State** Properties einer ParentView beliebig viele ChildViews heruntergereicht werden.
Das sollte jedoch vermieden werden, da der Code sonst schnell schlecht nachvollziehbar ist.

In so einem Fall wird ein globaler State angelegt, der jeder Komponente bekannt ist.
Dies wird mittels dem Property Wrapper **@EnvironmentObject** gemacht.
