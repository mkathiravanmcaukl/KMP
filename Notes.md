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

## 8. What is Kotlin Multiplatform Mobile (KMM)?

  Ans: Kotlin Multiplatform Mobile (KMM) is a framework within Kotlin Multiplatform designed specifically for sharing code between Android and iOS applications. It allows developers to write common code for business logic, networking, data storage, and other shared functionality while enabling platform-specific code for UI and platform-dependent features.

### Key Features of KMM:
1. **Code Sharing**:
   - Write shared code in the `commonMain` module for business logic, networking, and data models.
   - Use platform-specific modules (`androidMain`, `iosMain`) for platform-dependent implementations.

2. **Expect/Actual Mechanism**:
   - Use `expect` declarations in shared code and provide `actual` implementations in platform-specific modules.

3. **Interoperability**:
   - **Android**: Interoperates seamlessly with Java and Android libraries.
   - **iOS**: Produces native frameworks compatible with Swift and Objective-C.

4. **Gradle Integration**:
   - Configures targets, dependencies, and source sets for Android and iOS.

5. **Library Support**:
   - Supports multiplatform libraries like Ktor (networking), Kotlinx Serialization, and SQLDelight (database).

6. **Native Performance**:
   - Compiles to native binaries for iOS using Kotlin/Native and to JVM bytecode for Android.

### Benefits:
- Reduces code duplication between Android and iOS.
- Speeds up development by reusing shared logic.
- Ensures consistency across platforms.

KMM is ideal for teams building cross-platform mobile apps while maintaining native UI and platform-specific experiences.

## 9. How do you handle Database in Kotlin Multiplatform?

Ans: In Kotlin Multiplatform (KMP), databases are typically handled using libraries that support multiplatform development. One of the most popular libraries for this purpose is **SQLDelight**, which provides a multiplatform SQL-based database solution. Here's how you can handle databases in KMP:

---

### 1. **Add SQLDelight Dependency**
Add the SQLDelight plugin and dependencies to your `build.gradle.kts` file:

```kotlin
plugins {
    kotlin("multiplatform")
    id("com.squareup.sqldelight")
}

kotlin {
    jvm()
    ios()
    sourceSets {
        val commonMain by getting {
            dependencies {
                implementation("com.squareup.sqldelight:runtime:1.5.5")
            }
        }
        val androidMain by getting {
            dependencies {
                implementation("com.squareup.sqldelight:android-driver:1.5.5")
            }
        }
        val iosMain by getting {
            dependencies {
                implementation("com.squareup.sqldelight:native-driver:1.5.5")
            }
        }
    }
}

sqldelight {
    database("AppDatabase") {
        packageName = "com.example.database"
    }
}
```

---

### 2. **Define SQL Schema**
Create a `.sq` file (e.g., `AppDatabase.sq`) in the `commonMain` source set to define your database schema and queries:

```sql
CREATE TABLE User(
    id INTEGER NOT NULL PRIMARY KEY,
    name TEXT NOT NULL
);

selectAllUsers:
SELECT * FROM User;

insertUser:
INSERT INTO User(id, name) VALUES (?, ?);
```

---

### 3. **Generate Database Code**
SQLDelight generates type-safe Kotlin code for your database. You can access the database using the generated `AppDatabase` class.

---

### 4. **Initialize the Database**
Initialize the database in platform-specific code using the appropriate driver:

#### Android (`androidMain`):
```kotlin
import android.content.Context
import com.example.database.AppDatabase
import com.squareup.sqldelight.android.AndroidSqliteDriver

fun createDatabase(context: Context): AppDatabase {
    val driver = AndroidSqliteDriver(AppDatabase.Schema, context, "app.db")
    return AppDatabase(driver)
}
```

#### iOS (`iosMain`):
```kotlin
import com.example.database.AppDatabase
import com.squareup.sqldelight.native.NativeSqliteDriver

fun createDatabase(): AppDatabase {
    val driver = NativeSqliteDriver(AppDatabase.Schema, "app.db")
    return AppDatabase(driver)
}
```

---

### 5. **Use the Database in Common Code**
You can now use the database in the `commonMain` module:

```kotlin
class UserRepository(private val database: AppDatabase) {
    fun getAllUsers(): List<User> {
        return database.userQueries.selectAllUsers().executeAsList()
    }

    fun insertUser(id: Long, name: String) {
        database.userQueries.insertUser(id, name)
    }
}
```

---

This approach allows you to share database logic across platforms while using platform-specific drivers for database access.

## 10. How do you handle Networking (API calls) in Kotlin Multiplatform?

Ans: In Kotlin Multiplatform (KMP), networking is typically handled using **Ktor**, a multiplatform networking library. Ktor provides a client that works across platforms, allowing you to write shared code for making API calls.

### Steps to Handle Networking in KMP:

---

### 1. **Add Dependencies**
Add the Ktor client dependencies to your `build.gradle.kts` file:

```kotlin
kotlin {
    jvm()
    ios()
    sourceSets {
        val commonMain by getting {
            dependencies {
                implementation("io.ktor:ktor-client-core:2.3.4")
                implementation("io.ktor:ktor-client-content-negotiation:2.3.4")
                implementation("io.ktor:ktor-serialization-kotlinx-json:2.3.4")
            }
        }
        val androidMain by getting {
            dependencies {
                implementation("io.ktor:ktor-client-okhttp:2.3.4")
            }
        }
        val iosMain by getting {
            dependencies {
                implementation("io.ktor:ktor-client-darwin:2.3.4")
            }
        }
    }
}
```

---

### 2. **Create a Shared API Client**
Define a shared API client in the `commonMain` module:

```kotlin
import io.ktor.client.*
import io.ktor.client.call.*
import io.ktor.client.request.*
import io.ktor.client.statement.*
import io.ktor.client.plugins.contentnegotiation.*
import io.ktor.serialization.kotlinx.json.*
import kotlinx.serialization.Serializable

class ApiClient {
    private val client = HttpClient {
        install(ContentNegotiation) {
            json()
        }
    }

    suspend fun getExampleData(): ExampleResponse {
        val response: HttpResponse = client.get("https://api.example.com/data")
        return response.body()
    }
}

@Serializable
data class ExampleResponse(
    val id: Int,
    val name: String
)
```

---

### 3. **Initialize Platform-Specific Clients**
Ktor uses platform-specific engines. For example:
- **Android**: `OkHttp`
- **iOS**: `Darwin`

These engines are automatically selected based on the platform-specific dependencies added earlier.

---

### 4. **Use the API Client in Shared Code**
You can now use the `ApiClient` in your shared code:

```kotlin
class ExampleRepository(private val apiClient: ApiClient) {
    suspend fun fetchData(): ExampleResponse {
        return apiClient.getExampleData()
    }
}
```

---

This approach allows you to write shared networking logic while leveraging Ktor's multiplatform capabilities.
