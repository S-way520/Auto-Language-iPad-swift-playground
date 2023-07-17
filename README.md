# Auto-Language-iPad-swift-playground
It changes with the change of system language.
# The first part
![IMG_0313](https://github.com/S-way520/Auto-Language-iPad-swift-playground/assets/95877651/3763f774-5426-4ca3-b478-1e9b0cba5af7)
# The second part
Code file 1
```swift
import SwiftUI
@main
struct MyApp: App {
    @State var language: String = LanguageManager.shared.getAppLanguage()
    var body: some Scene {
        WindowGroup {
            ZStack{
                ContentView()
                    .onReceive(NotificationCenter.default.publisher(for: NSLocale.currentLocaleDidChangeNotification)) { _ in
                        let newLanguage = LanguageManager.shared.getAppLanguage()
                        if newLanguage != language {
                            language = newLanguage
                        }
                    }
                    .environment(\.locale, .init(identifier: language)) 
            }
        }
    }
}
class LanguageManager {
    // MARK: - Properties
    static let shared = LanguageManager()
    private let defaults = UserDefaults.standard
    // MARK: - Public Methods
    func setAppLanguage(to language: String) {
        defaults.set([language], forKey: "AppleLanguages")
        defaults.synchronize()
    }
    func getAppLanguage() -> String {
        guard let languages = defaults.object(forKey: "AppleLanguages") as? [String],
              let language = languages.first else {
            return "en"
        }
        return language
    }
}
```
Code file 2
```swift
import SwiftUI
struct ContentView: View {
    @Environment(\.locale) var locale
    var languageCode: String? {locale.language.languageCode?.identifier}
    var isChineseLanguage: Bool { languageCode == "zh"}
    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(.accentColor)
            if isChineseLanguage {
                Text("你好，世界！")
            } else {
                Text("Hello, world!")
            }
        }
    }
}
```
