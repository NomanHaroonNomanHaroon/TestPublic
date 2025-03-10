import UIKit

    func redesignCaptionText(
        text: String,
        color: UIColor = .gray,
        font: UIFont = UIFont.systemFont(ofSize: 16),
        alignment: NSTextAlignment = .left,
        lineHeight: CGFloat = 1.2,
        boldSubstrings: [String]? = nil // Optional parameter for bold substrings
    ) -> NSAttributedString {
        
        let paragraphStyle = NSMutableParagraphStyle()
        paragraphStyle.alignment = alignment
        paragraphStyle.lineSpacing = lineHeight

        let attributes: [NSAttributedString.Key: Any] = [
            .foregroundColor: color,
            .font: font,
            .paragraphStyle: paragraphStyle
        ]

        let attributedString = NSMutableAttributedString(string: text, attributes: attributes)

        if let boldSubstrings = boldSubstrings {
            let boldFont = UIFont.boldSystemFont(ofSize: font.pointSize)

            for substring in boldSubstrings {
                let range = (text as NSString).range(of: substring)
                if range.location != NSNotFound {
                    var existingAttributes = attributedString.attributes(at: range.location, effectiveRange: nil)
                    existingAttributes[.font] = boldFont
                    attributedString.addAttributes(existingAttributes, range: range)
                }
            }
        }

        return attributedString
    }
