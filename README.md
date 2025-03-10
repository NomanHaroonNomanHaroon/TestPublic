var sentenceCapitalized: String {
        let sentences = self.components(separatedBy: ". ")
        let capitalizedSentences = sentences.map { sentence -> String in
            let predicate = NSPredicate(format: "SELF MATCHES %@", "^[a-z].*")
            if predicate.evaluate(with: sentence) {
                return sentence.prefix(1).uppercased() + sentence.dropFirst()
            }
            return sentence
        }
        return capitalizedSentences.joined(separator: ". ")
    }
