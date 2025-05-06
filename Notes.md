# KMP Interview Questions and Answers

## 1. What is Kotlin Multiplatform?

   Ans: Kotlin Multiplatform (KMP) is a feature of the Kotlin programming language that allows developers to write code that can be shared across multiple platforms, such as Android, iOS, web, and desktop, while still enabling platform-specific code where necessary. It is part of Kotlin's effort to support cross-platform development.

### Key Features:
1. **Code Sharing**: Write common business logic, data models, and other shared code once and reuse it across platforms.
2. **Platform-Specific Code**: Use platform-specific APIs and features when needed by writing platform-specific implementations.
3. **Gradual Adoption**: You can adopt Kotlin Multiplatform incrementally in existing projects.
4. **Interoperability**: Works seamlessly with existing platform-specific codebases (e.g., Java for Android, Swift/Objective-C for iOS).

### Architecture:
- **Common Module**: Contains shared code (e.g., business logic, networking, etc.).
- **Platform-Specific Modules**: Contain code specific to each platform (e.g., Android, iOS).
- **Expect/Actual Mechanism**: Allows you to define "expected" declarations in the common module and provide "actual" implementations in platform-specific modules.

### Supported Platforms:
- **Mobile**: Android, iOS
- **Web**: JavaScript
- **Desktop**: JVM, macOS, Windows, Linux
- **Embedded**: Native platforms via Kotlin/Native

### Example Use Case:
You can use Kotlin Multiplatform to write a shared library for networking or data storage logic that works on both Android and iOS, while still using platform-specific UI frameworks like Jetpack Compose for Android and SwiftUI for iOS.

### Benefits:
- Reduces code duplication.
- Simplifies maintenance and testing.
- Allows leveraging Kotlin's modern language features across platforms.

Kotlin Multiplatform is particularly useful for teams looking to share code across platforms while maintaining flexibility for platform-specific needs.


## 2. Why do we use Kotlin Multiplatform?

  Ans: Kotlin Multiplatform (KMP) is used to enable **code sharing** across multiple platforms while allowing platform-specific implementations where necessary. It helps developers write common logic once and reuse it across platforms like Android, iOS, web, and desktop, reducing duplication and improving maintainability.

### Key Reasons to Use Kotlin Multiplatform:
1. **Code Reusability**: Share business logic, data models, and other non-UI code across platforms, reducing development time and effort.
2. **Platform-Specific Flexibility**: Write platform-specific code for UI or other platform-dependent features while keeping shared logic in a common module.
3. **Improved Maintainability**: Centralize shared logic, making it easier to maintain and test.
4. **Interoperability**: Works seamlessly with existing platform-specific codebases (e.g., Java for Android, Swift for iOS).
5. **Gradual Adoption**: Can be integrated incrementally into existing projects without requiring a complete rewrite.
6. **Cost Efficiency**: Reduces the cost of developing and maintaining separate codebases for each platform.

### Use Cases:
- Sharing business logic (e.g., networking, data storage, algorithms) between Android and iOS.
- Building cross-platform libraries or SDKs.
- Simplifying multi-platform application development while maintaining native UI experiences.
