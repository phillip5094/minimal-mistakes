---
title: "Swift toast message"
categories: 
  - iOS
  - Swift
last_modified_at: 2021-05-29T13:00:00+09:00

toc: true
---
2021.05.29

### 스위프트 토스트 메시지 출력

```swift
extension UIViewController {
    static func showToastMessage(_ message: String, font: UIFont = UIFont.systemFont(ofSize: 12, weight: .light)) {
        let window = UIApplication.shared.windows.first!
        let toastLabel = UILabel(frame: CGRect(x: window.frame.width / 2 - 150, y: window.frame.height - 150, width: 300, height: 60))
        
        toastLabel.backgroundColor = UIColor.black.withAlphaComponent(0.7)
        toastLabel.textColor = UIColor.white
        toastLabel.numberOfLines = 2
        toastLabel.font = font
        toastLabel.text = message
        toastLabel.textAlignment = .center
        toastLabel.layer.cornerRadius = 10
        toastLabel.clipsToBounds = true
        
        guard let topController = UIApplication.shared.keyWindow?.rootViewController else { return }
        topController.view.addSubview(toastLabel)

        UIView.animate(withDuration: 1.5, delay: 0.3, options: .curveEaseOut) {
            toastLabel.alpha = 0.0
        } completion: { _ in
            toastLabel.removeFromSuperview()
        }
    }
}
```