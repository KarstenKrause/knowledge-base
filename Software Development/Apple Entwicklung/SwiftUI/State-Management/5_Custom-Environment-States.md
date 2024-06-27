# Custom Environment States

Häufig werden globale States oder Flags benötigt, die in der gesamten App zu jeder Zeit in jeder View zu Verfügung stehen müssen.

Z.B. muss in der TimeChisel iOS App gewährleistet sein, dass während einer Zeiterfassung das Bearbeiten oder das Löschen von Jobs unterbunden wird. Ohne dem würde es während einer Zeiterfassung für den Job zu Konflikten kommen, wenn diese gleichzeitig bearbeitet, oder gelöscht werden.

Folgende Schritte werden benötigt, um eine eingenen Environment State anzulegen:

## 1. @Observable Klasse erstellen

```swift
import SwiftUI
import Observation

@Observable
class TimeTrackingStatus {
    var isTracking: Bool = false
}
```

- **@Observable:** Dies markiert die Klasse als beobachtbar.

- **TimeTrackingStatus:** Dies ist die Klasse, die die globale State-Variable isTracking enthält.

- **isTracking:** Eine einfache Bool-Variable, die den Zustand des Trackings repräsentiert. Standardmäßig ist sie auf false gesetzt.
  
## 2. EnvironmentKey Struct erstellen

```swift
struct TimeTrackingStatusKey: EnvironmentKey {
    static let defaultValue: TimeTrackingStatus = TimeTrackingStatus()
}
```

- **TimeTrackingStatusKey:** Dies ist eine Struktur, die das EnvironmentKey Protokoll implementiert. EnvironmentKey wird verwendet, um einen Schlüssel für die Environment-Variable zu definieren.

- **defaultValue:** Die standardmäßige Instanz von TimeTrackingStatus, die verwendet wird, wenn kein anderer Wert gesetzt ist. Dies ist erforderlich, da Environment-Variablen einen Standardwert haben müssen.

## 3. Extension von EnvironmentValues

```swift
extension EnvironmentValues {
    var timeTrackingStatus: TimeTrackingStatus {
        get { self[TimeTrackingStatusKey.self] }
        set { self[TimeTrackingStatusKey.self] = newValue }
    }
}
```
- **Erweiterung von EnvironmentValues:** Dies fügt eine neue Property zu schon vorhandenen EnvironmentValues hinzu.
  
- **timeTrackingStatus:** Eine computed Property, die TimeTrackingStatus verwendet. Der Getter und Setter ermöglichen es, auf den Wert zuzugreifen und ihn zu ändern.


## 4. Verwendung
Nun kann der Environment state über den @Environment-Wrapper verwendet werden:

```swift
...

@Environment(\.timeTrackingStatus) var trackingStatus

...
```

```swift
...

Button(action: {
    if trackingStatus.isTracking {
      showingTimeIsTrackingAlert.toggle()
    } else {
      showingDeleteAlert = true
      jobToDelete = job
    }
}, label: {
    Text("Löschen")
})
...
```
