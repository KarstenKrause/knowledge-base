# Statemanagement

## @Observable View-Models

Ein ViewModel repräsentiert den Zustand und das Verhalten einer SwiftUI-Ansicht.
Mit iOS 17 wurde das @Observable Macro eingeführt und ersetzt die vorherige Implementierung des ObservableObject Protocols. Dadurch müssen u.A. die States der ViewModel-Klasse nicht mehr mit dem Macro @Published markiert werden, was die Lesbarkeit verbessert und die Performance erhöht.

Mehr Details in der [offiziellen Dokumentation](https://developer.apple.com/documentation/swiftui/migrating-from-the-observable-object-protocol-to-the-observable-macro)

### Beispiel

```swift
import SwiftUI
import Observation

@Observable class JobViewModel {
    // Aktuell müssen Initialwerte angegeben werden
    var companyName = ""
    var jobTitle = ""
    var hourlyWage = ""
    var workingHoursPerWeek = 1
    var workingDaysPerWeek = 1
    var pauseMinutesPerDay = 0
    
    
    init(companyName: String, jobTitle: String, hourlyWage: String, workingHoursPerWeek: Int, workingDaysPerWeek: Int, pauseMinutesPerDay: Int) {
        self.companyName = companyName
        self.jobTitle = jobTitle
        self.hourlyWage = hourlyWage
        self.workingHoursPerWeek = workingHoursPerWeek
        self.workingDaysPerWeek = workingDaysPerWeek
        self.pauseMinutesPerDay = pauseMinutesPerDay
    }
}
```

## @Bindable

Sollen States eines ViewModels in einer View gebindet werden, muss das ViewModel-Objekt mit dem Macro **@Bindable** markiert.

```swift
@Bindable var jobVM = JobViewModel(
        companyName: "",
        jobTitle: "",
        hourlyWage: "",
        workingHoursPerWeek: 1,
        workingDaysPerWeek: 1,
        pauseMinutesPerDay: 0
    )
    
    var body: some View {
        NavigationView {
            
            Form {
                Section("Jobinfos") {
                    TextField("Name des Unternehmens", text: $jobVM.companyName)
                    TextField("Job-Titel", text: $jobVM.companyName)
                    TextField("Stundenlohn", text: $jobVM.hourlyWage)
                }
                ...
```
