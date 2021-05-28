---
title: "Swift get method list"
categories: 
  - iOS
  - Swift
last_modified_at: 2021-05-29T13:00:00+09:00

toc: true
---
2021.05.29

### 스위프트로 인스턴스 메서드 목록 얻기

* 다만, 인스턴스 메서드 선언할 때 `@objc` annotation 을 붙여준 것들만 읽어준다.

```swift
extension NSObject {
    static func methodList(className: String) -> [String]? {
        guard let someClass = NSClassFromString(className) else { return nil }
        return self.methodList(cls: someClass)
    }
    
    static func methodList(cls: AnyClass) -> [String]? {
        var methodCount: UInt32 = 0
        let methodList = class_copyMethodList(cls, &methodCount)
        
        var result = [String]()
        
        if let methodList = methodList, methodCount > 0 {
            enumerateCArray(array: methodList, count: methodCount) { i, m in
                let name = methodName(m: m) ?? "unknown"
                result.append(name)
            }
            free(methodList)
        }
        
        return result
    }
}

// MARK: private
private extension NSObject {
    static func enumerateCArray<T>(array: UnsafePointer<T>, count: UInt32, f: (UInt32, T) -> Void) {
        var ptr = array
        for i in 0..<count {
            f(i, ptr.pointee)
            ptr = ptr.successor()
        }
    }
    
    static func methodName(m: Method) -> String? {
        let sel = method_getName(m)
        let nameCString = sel_getName(sel)
        return String(cString: nameCString)
    }
}

```