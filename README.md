
```
func boldSubstrings(_ substrings: [String], fontSize: CGFloat = 16) -> NSAttributedString {
        let attributedString = NSMutableAttributedString(string: self)
        let boldFont = UIFont.boldSystemFont(ofSize: fontSize)

for substring in substrings {
            let range = (self as NSString).range(of: substring)
            if range.location != NSNotFound {
                attributedString.addAttribute(.font, value: boldFont, range: range)
            }
        }
        return attributedString
    }

```
