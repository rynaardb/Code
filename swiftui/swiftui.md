# SwiftUI

## UIKit

### Wrapping UIKit Views

UIKit or custom UIViews can be presented using the `UIViewRepresentable` protocol.

```swift
struct ContentView: View {
    @State var isLoading = false

    var body: some View {
        Activity(style: .large, isAnimating: $isLoading)
    }
}

struct Activity : UIViewRepresentable {
    let style: UIActivityIndicatorView.Style
    @Binding var isAnimating: Bool

    init(style: UIActivityIndicatorView.Style = .medium, isAnimating: Binding<Bool>) {
        self.style = style
        self._isAnimating = isAnimating
    }

    func makeUIView(context: Context) -> UIActivityIndicatorView {
        let indicator = UIActivityIndicatorView(style: style)
        indicator.hidesWhenStopped = true
        return indicator
    }

    func updateUIView(_ uiView: UIActivityIndicatorView, context: Context) {
        if isAnimating {
            uiView.startAnimating()
        } else {
            uiView.stopAnimating()
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

## State

### Preferences

Using preferences you can push state values from child to parent views.

The example below creates a circle relative to the width of the label:

```swift
struct ContentView: View {
    var body: some View {
        Label(message: "Swift")
    }
}

struct Label: View {
    @State var message: String
    @State
    private var size: CGSize?

    var body: some View {
        Circle()
            .fill(Color.blue)
            .frame(width: self.size?.width, height: self.size?.width)
            .overlay(
                Text(message)
                .font(.title)
                .foregroundColor(.white)
                .padding(5)
                .overlay(GeometryReader { geometry in
                    Color.clear
                        .preference(
                            key: LabelSizePreference.self,
                            value: geometry.size)
                })
            )
            .onPreferenceChange(LabelSizePreference.self, perform: { size in
                self.size = size
            })
    }
}

struct LabelSizePreference: PreferenceKey {
    static var defaultValue: CGSize?

    static func reduce(value: inout CGSize?, nextValue: () -> CGSize?) {
        value = value ?? nextValue()
    }
}
```

