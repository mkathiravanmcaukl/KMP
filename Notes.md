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

## 3. What parts of an app can be shared using Kotlin Multiplatform?

  Ans: Using Kotlin Multiplatform (KMP), the following parts of an app can be shared across platforms:

1. **Business Logic**:
   - Core application logic, such as data processing, algorithms, and validation.

2. **Networking**:
   - API calls, HTTP requests, and response handling using libraries like Ktor.

3. **Data Models**:
   - Shared data classes, DTOs, and serialization logic (e.g., using Kotlinx Serialization).

4. **Persistence**:
   - Database access and storage logic using libraries like SQLDelight or shared preferences.

5. **Dependency Injection**:
   - Shared DI setup using libraries like Koin or manual DI.

6. **State Management**:
   - Shared state management logic for handling application state.

7. **Utilities**:
   - Common utility functions, extensions, and helper classes.

8. **Shared Libraries**:
   - Reusable libraries or SDKs for specific functionality.

9. **Testing**:
   - Unit tests for shared code.

Platform-specific code (e.g., UI, platform APIs) is implemented separately in platform-specific modules.

## 4. How does Kotlin Multiplatform actually work under the hood?

   Ans: Kotlin Multiplatform (KMP) works by leveraging Kotlin's ability to compile to multiple targets, such as JVM, JavaScript, and native binaries, while providing mechanisms to share common code and implement platform-specific functionality where needed. Here's how it works under the hood:

### 1. **Compilation Targets**
Kotlin Multiplatform supports multiple compilation targets:
   - **JVM**: For Android and other JVM-based platforms.
   - **JavaScript**: For web applications.
   - **Native**: For iOS, macOS, Windows, Linux, and other native platforms using Kotlin/Native.

Each target has its own compiler backend that generates platform-specific binaries or code.

### 2. **Common Code and Platform-Specific Code**
   - **Common Code**: Shared code is written in the `commonMain` source set. This code is compiled for all targets and can only use APIs available in the Kotlin standard library or libraries designed for multiplatform use.
   - **Platform-Specific Code**: Code that depends on platform-specific APIs is written in target-specific source sets (e.g., `androidMain`, `iosMain`). These source sets can use platform-specific libraries and features.

### 3. **Expect/Actual Mechanism**
KMP uses the `expect`/`actual` mechanism to handle platform-specific implementations:
   - **`expect` Declaration**: Defines an API in the `commonMain` source set without providing an implementation.
   - **`actual` Implementation**: Provides the platform-specific implementation in the target-specific source sets.

Example:
```kotlin
// commonMain
expect fun getPlatformName(): String

// androidMain
actual fun getPlatformName(): String = "Android"

// iosMain
actual fun getPlatformName(): String = "iOS"
```

### 4. **Gradle Multiplatform Plugin**
The Gradle Multiplatform plugin (`kotlin-multiplatform`) is used to configure the project. It defines the targets, source sets, and dependencies for each platform.

Example:
```kotlin
kotlin {
    jvm()
    ios()
    js()

    sourceSets {
        val commonMain by getting {
            dependencies {
                implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core")
            }
        }
        val androidMain by getting
        val iosMain by getting
    }
}
```

### 5. **Interop with Platform Code**
   - **JVM**: Interoperates with Java and Android libraries.
   - **iOS**: Generates Objective-C/Swift-compatible frameworks.
   - **JavaScript**: Produces JavaScript modules.

### 6. **Library Support**
KMP supports libraries designed for multiplatform use, such as Ktor (networking), Kotlinx Serialization (serialization), and SQLDelight (database).

### 7. **Output**
The Kotlin compiler produces platform-specific artifacts:
   - **JVM**: `.jar` or `.aar` files.
   - **iOS**: Native frameworks.
   - **JavaScript**: `.js` files.

This architecture allows developers to share code across platforms while maintaining platform-specific flexibility.

## 5. What are the main modules or projects you find in a KMP setup?

  Ans: In a Kotlin Multiplatform (KMP) setup, the project is typically divided into the following main modules or projects:

### 1. **Common Module**
   - Contains shared code that is platform-agnostic.
   - Includes business logic, data models, networking, and other reusable components.
   - Uses the `commonMain` source set.
   - Example: `:shared`

### 2. **Platform-Specific Modules**
   - Contain platform-specific code and implementations.
   - Use source sets like `androidMain`, `iosMain`, or `jsMain`.
   - Examples:
     - `:androidApp` for Android-specific code.
     - `:iosApp` for iOS-specific code.
     - `:webApp` for JavaScript or web-specific code.

### 3. **Expect/Actual Implementations**
   - Platform-specific modules provide `actual` implementations for `expect` declarations in the common module.
   - Example:
     - `expect fun getPlatformName(): String` in `commonMain`.
     - `actual fun getPlatformName(): String = "Android"` in `androidMain`.

### 4. **Gradle Multiplatform Plugin**
   - Configures the targets and dependencies for each module.
   - Example:
     ```kotlin
     kotlin {
         jvm()
         ios()
         js()
         sourceSets {
             val commonMain by getting
             val androidMain by getting
             val iosMain by getting
         }
     }
     ```

### 5. **Testing Modules**
   - Shared test code in `commonTest`.
   - Platform-specific tests in `androidTest`, `iosTest`, etc.

This modular structure ensures code reusability while allowing platform-specific flexibility.

## 6. What are expect/actual keywords in Kotlin Multiplatform?

 Ans: In Kotlin Multiplatform, the `expect` and `actual` keywords are used to define and implement platform-specific functionality while keeping the shared code in the common module.

### **`expect` Keyword**
- Used in the **common module** to declare a platform-specific API or functionality.
- It defines the expected behavior or contract without providing an implementation.
- Example:
  ```kotlin
  // commonMain
  expect fun getPlatformName(): String
  ```

### **`actual` Keyword**
- Used in **platform-specific modules** to provide the actual implementation of the `expect` declaration.
- Each platform-specific module must provide its own `actual` implementation.
- Example:
  ```kotlin
  // androidMain
  actual fun getPlatformName(): String = "Android"

  // iosMain
  actual fun getPlatformName(): String = "iOS"
  ```

### How It Works
1. The `expect` declaration in the common module defines the API that the shared code can use.
2. The `actual` implementations in platform-specific modules fulfill the contract for each target platform.

This mechanism allows you to write shared code in the common module while handling platform-specific differences in the respective platform modules.

## 7. What is Kotlin/Native?

Ans: Kotlin/Native is a technology within the Kotlin ecosystem that allows Kotlin code to be compiled into native binaries, enabling it to run on platforms where a virtual machine (like the JVM) is not available. It is part of Kotlin Multiplatform and is primarily used for targeting platforms such as iOS, macOS, Windows, Linux, and embedded systems.

### Key Features of Kotlin/Native:
1. **Native Binaries**: Compiles Kotlin code into platform-specific native executables.
2. **Interoperability**: Provides seamless interoperability with platform-specific languages like Swift/Objective-C (for iOS/macOS) and C/C++.
3. **Garbage Collection**: Uses a custom memory management model instead of relying on JVM garbage collection.
4. **Multithreading**: Supports multithreading with a focus on immutability and thread-local data to ensure safety.
5. **No Virtual Machine**: Runs directly on the target platform without requiring a JVM or other runtime.

### Use Cases:
- Developing iOS applications alongside Android in a Kotlin Multiplatform project.
- Writing cross-platform libraries or tools.
- Targeting platforms like embedded systems or desktop environments.

Kotlin/Native is powered by the LLVM compiler infrastructure, which generates highly optimized native code for the target platform.
