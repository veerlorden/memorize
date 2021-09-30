# Memorize 🎮
#### iOS memory card game based on Stanford's CS193p course on SwiftUI (Spring 2021)

some gif

## About the game
Your task is to turn over all the cards one by one and find the same ones. There are only two identical cards in each deck.
When you find a match, you get a point for each identical card. You get +1 extra point if you do it before card animation ends.
The app has some default themes, but you are free to add new themes and edit current ones: add or remove emojis, change theme color or number of cards in the deck. 

## Technologies
- Swift
- SwiftUI
- MVVM
- Animation

## Screenshots

some screenshots

## Sample code
```swift
class ThemeStore: ObservableObject {
    let name: String
    
    @Published var themes: [Theme] = [] {
        didSet {
            storeInUserDefaults()
        }
    }
    
    private var userDefaultsKey: String {
        "Theme Store:" + name
    }
    
    private func storeInUserDefaults() {
        UserDefaults.standard.set(try? JSONEncoder().encode(themes), forKey: userDefaultsKey)
    }
    
    private func restoreFromUserDefaults() {
        if let jsonData = UserDefaults.standard.data(forKey: userDefaultsKey),
           let decodedThemes = try? JSONDecoder().decode([Theme].self, from: jsonData) {
            themes = decodedThemes
        }
    }
    
    private func templateThemes() {
        insertTheme(titled: "Halloween", emojis: ["👻", "🎃", "🕷️", "🍬", "💀"], color: .orange)
        insertTheme(titled: "Christmas", emojis: ["🎅", "⛪", "🌟", "❄️", "⛄", "🎄", "🎁", "🧦"], color: .blue)
        insertTheme(titled: "Transport", emojis: ["🚗", "🚕", "🚙", "🚌", "🚎", "🏎", "🚓", "🚑", "🚒", "🚐", "🛻", "🚚", "🚛", "🚜", "🛵", "🛺", "🚔", "🚍", "🚘", "🚖", "✈️", "🚝", "🚢", "🚁"], color: .yellow, numberOfPairsOfCards: 10)
        insertTheme(titled: "Sports", emojis: ["⚽️", "🏀", "🏈", "⚾️", "🥎", "🎾", "🏐", "🏉", "🎱", "🥏", "🪀", "🏓", "🥊", "🥅", "🥌", "⛸", "🥋"], color: .purple)
        insertTheme(titled: "Animals", emojis: ["🐶", "🐱", "🐭", "🐹", "🐰", "🦊", "🐻", "🐼", "🐻‍❄️", "🐨", "🐯", "🦁", "🐮", "🐷", "🐸", "🐵"], color: .green)
    }
    
    init(named name: String) {
        self.name = name
        restoreFromUserDefaults()
        if themes.isEmpty {
            templateThemes()
        }
    }
    
    // MARK: - Intents
    
    func theme(at index: Int) -> Theme {
        let safeIndex = min(max(0, index), themes.count - 1)
        return themes[safeIndex]
    }
    
    @discardableResult
    func insertTheme(titled title: String,
                     emojis: [String] = [],
                     at index: Int = 0,
                     color: Color = .red,
                     numberOfPairsOfCards: Int? = nil) -> Theme {
        let unique = (themes.max(by: { $0.id < $1.id })?.id ?? 0) + 1
        let theme = Theme(title: title,
                          emojis: emojis,
                          id: unique,
                          color: color,
                          numberOfPairsOfCards: numberOfPairsOfCards ?? emojis.count)
        let safeIndex = max(min(0, index), themes.count)
        themes.insert(theme, at: safeIndex)
        return themes[safeIndex]
    }
    
    @discardableResult
    func removeTheme(at index: Int) -> Int {
        if themes.count > 1, themes.indices.contains(index) {
            themes.remove(at: index)
        }
        return index % themes.count
    }
}
```

## Credits
[Stanford University's course CS193p (Developing Applications for iOS using SwiftUI)](https://cs193p.sites.stanford.edu)
