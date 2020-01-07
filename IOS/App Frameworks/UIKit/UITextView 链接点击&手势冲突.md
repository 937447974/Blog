**1 [链接点击](#1)**

**2 [手势冲突解决](#2)**

----

# <a id="1"/>1 链接点击

UItextView 初始化

```swift
lazy var textView: UILinkTextView = {
    let view = UILinkTextView(frame: CGRect(x: 60, y: 0, width: ScreenWidth-80, height: 0))
    view.linkTextAttributes = [.foregroundColor: UIColor.hex(0x3B5998)]
    view.textContainerInset = UIEdgeInsets()
    view.isEditable = false
    view.delegate = self
    view.isScrollEnabled = false
    return view
}()
    
func textView(_ textView: UITextView, shouldInteractWith URL: URL, in characterRange: NSRange) -> Bool {
    UIApplication.shared.openURL(URL)
    return false
}
    
@available(iOS 10.0, *)
func textView(_ textView: UITextView, shouldInteractWith URL: URL, in characterRange: NSRange, interaction: UITextItemInteraction) -> Bool {
    UIApplication.shared.openURL(URL)
    return false
}

```

链接点击数据解析

```swift
class Model {
    
    var attributedText: NSAttributedString!
    var isUserInteractionEnabled: Bool
    var selectRanges = [NSRange]()
    
    init(text: String) {
        let attributedText = NSMutableAttributedString(string: text, attributes: [.font: UIFont.font17, .foregroundColor: UIColor.title])
        let regulaStr = "(((https?|ftp)://)|(www(\\.)){1})[-A-Za-z0-9+&@#/%?=~_|!:,;]+(\\.){1}[-A-Za-z0-9+&@#/:%=~_|.?]+"
        let regex = try! NSRegularExpression(pattern: regulaStr, options: .caseInsensitive)
        let matches = regex.matches(in: text, options: NSRegularExpression.MatchingOptions(rawValue: 0), range: NSRange(location: 0, length: text.count))
        for match in matches {
            let range = match.range
            self.selectRanges.append(range)
            var value = (text as NSString).substring(with: range)
            if !value.hasPrefix("http") { value = "https://" + value }
            attributedText.addAttribute(.link, value: value, range: range)
        }
        self.attributedText = attributedText
        self.isUserInteractionEnabled = matches.count > 0
    }
    
}
```

链接点击填充

```swift
self.textView.attributedText = cm.attributedText
self.textView.isUserInteractionEnabled = cm.isUserInteractionEnabled
self.textView.addSelectRanges(cm.selectRanges)
```

# <a id="2"/>2 手势冲突解决

手势冲突使用 UILinkTextView 解决

```swift
import UIKit

/// 响应链接的 TextView
open class UILinkTextView: UITextView {
    
    var selectRects = [CGRect]()
    
    open override func hitTest(_ point: CGPoint, with event: UIEvent?) -> UIView? {
        let view = super.hitTest(point, with: event)
        guard view == self else { return view }
        for rect in self.selectRects {
            if rect.contains(point) {
                return view
            }
        }
        return nil
    }
    
    open override func canPerformAction(_ action: Selector, withSender sender: Any?) -> Bool {
        self.resignFirstResponder()
        UIMenuController.shared.isMenuVisible = false
        return false
    }
    
    open func addSelectRanges(_ selectRanges: [NSRange]) {
        self.selectRects.removeAll()
        for range in selectRanges {
            let beginning = self.beginningOfDocument
            let startPosition = self.position(from: beginning, offset: range.location)!
            let endPosition = self.position(from: beginning, offset: range.location + range.length)!
            let textRange = self.textRange(from: startPosition, to: endPosition)!
            let selectionRects = self.selectionRects(for: textRange)
            for selectionRect in selectionRects {
                self.selectRects.append(selectionRect.rect)
            }
        }
    }
    
}
```

&#160;

----------

# 其他

## 版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog

