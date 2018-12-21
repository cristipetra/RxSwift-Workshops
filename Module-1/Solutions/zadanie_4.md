# Zadanie 4

### Dany jest skończony strumień słów (Stringi), które utworzą jedno zdanie.
### - pamiętaj że zdania rozpoczynamy z dużej litery
### - pamiętaj o spacjach pomiedzy słowami
### - zdanie zakończ kropką (bezpośrednio po ostatnim słowie)

```swift

extension String {
    func capitalizingFirstLetter() -> String {
        return prefix(1).uppercased() + dropFirst()
    }
}

let words = PublishSubject<String>()
let expectedResult = "Ala ma kota."
let resultObserver = TestObserver<String>()

words
    .reduce([String]()) { $0 + [$1] }
    .map { words in
        words
            .joined(separator: " ")
            .capitalizingFirstLetter()
            .appending(".")
    }
    .test(using: resultObserver)
    .subscribe()

words.onNext("ala")
words.onNext("ma")
words.onNext("kota")
words.onCompleted()

resultObserver.assert(valuesEqualTo: ["Ala ma kota."])

```