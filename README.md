# CleanSleep App

## Overview
CleanSleep is a SwiftUI-based iOS application that helps users determine the optimal bedtime based on their desired wake-up time, sleep duration, and daily coffee intake. The app utilizes CoreML to make predictions for sleep recommendations.

## Features
- **Wake-up Time Selection**: Users can select their desired wake-up time using a `DatePicker`.
- **Sleep Duration Input**: A `Stepper` allows users to adjust their desired sleep amount between 4 to 12 hours.
- **Coffee Intake Tracking**: Users can specify their daily coffee intake with a `Stepper`.
- **Bedtime Calculation**: The app uses a CoreML model (`SleepCalculator`) to predict the ideal bedtime based on the provided inputs.
- **Alert System**: Displays the calculated bedtime recommendation using an alert.

## UI Components
The app follows a simple and intuitive UI layout with:
- `NavigationView` for structured navigation.
- `Form` to organize user input sections.
- Custom styling with defined colors.

## Code Breakdown

### Initialization
```swift
init(){
    UINavigationBar.appearance().largeTitleTextAttributes = [.foregroundColor: UIColor.white]
    UITableView.appearance().backgroundColor = .clear
}
```
- Sets up navigation bar appearance.
- Clears the default background color of `UITableView`.

### UI Elements
#### Wake-up Time Selection
```swift
@State private var WakeUp = defaultWakeUp

static var defaultWakeUp : Date{
    var components = DateComponents()
    components.hour = 8
    components.minute = 0
    return Calendar.current.date(from: components) ?? Date.now
}
```
- Defines a default wake-up time (8:00 AM).
- Allows users to set their preferred wake-up time via a `DatePicker`.

#### Sleep Amount and Coffee Intake
```swift
@State private var sleepAmount = 8.0
@State private var coffeeAmount = 1
```
- Tracks the desired sleep hours and daily coffee consumption.
- Users can adjust these values using `Stepper` controls.

### Bedtime Calculation
```swift
func CalculateBedtime(){
    do{
        let config = MLModelConfiguration()
        let model = try SleepCalculator(configuration: config)
        
        let components = Calendar.current.dateComponents([.hour, .minute], from: WakeUp)
        let hour = (components.hour ?? 0) * 60 * 60
        let minute = (components.minute ?? 0) * 60
        
        let prediction = try model.prediction(wake: Double(hour + minute), estimatedSleep: sleepAmount, coffee: Double(coffeeAmount))
        
        let sleepTime = WakeUp - prediction.actualSleep
        alertTitle = "Clean Sleep suggests that"
        alertMessage = "You should be in bed by \(sleepTime.formatted(date: .omitted, time: .shortened))"
    } catch{
        alertTitle = "Error"
        alertMessage = "Oops! There was a problem calculating your bedtime"
    }
    showAlert = true
}
```
- Loads the CoreML model (`SleepCalculator`).
- Converts wake-up time into seconds.
- Feeds user inputs (wake time, sleep amount, coffee intake) into the model.
- Calculates and displays the recommended bedtime.

## Preview
```swift
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

#Preview {
    ContentView()
}
```
- Provides a preview for SwiftUI.

## Dependencies
- **CoreML**: Used for sleep prediction.
- **SwiftUI**: Provides the user interface framework.

## Future Enhancements
- Improve UI with animations.
- Allow users to save their preferred settings.
- Provide more detailed sleep recommendations.

## Author
**Omid Shojaeian Zanjani**  
Created on **March 8, 2025**

