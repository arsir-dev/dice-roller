# 🎲 Dice Roller App
**Jetpack Compose + Clean Architecture + MVVM + Hilt + Unit/UI Tests**

---

## 📖 Overview
Dice Roller adalah aplikasi Android sederhana namun dirancang dengan **arsitektur modern dan praktik industri terbaik**.  
Aplikasi ini memungkinkan pengguna untuk menekan tombol untuk *melempar dadu* dan mendapatkan angka acak antara 1 hingga 6.  

---

## 🧩 Tech Stack

| Layer | Library / Teknologi | Deskripsi |
|-------|----------------------|-----------|
| **UI** | Jetpack Compose, Material3 | UI deklaratif + animasi modern |
| **Presentation** | ViewModel, StateFlow | Manajemen state reaktif |
| **Domain** | Use Case, Repository Pattern | Business logic yang terisolasi |
| **Data** | Random Generator (mock), dapat diganti ke API/DB | Abstraksi data |
| **DI** | Hilt | Dependency Injection |
| **Testing** | JUnit, Compose UI Test | Unit & UI Testing |

---

## 🧠 Architecture

Struktur mengikuti pola **Clean Architecture + MVVM**

```
app/
 ├── data/
 │    └── repository/
 │         └── DiceRepositoryImpl.kt
 ├── domain/
 │    ├── model/
 │    │    └── DiceResult.kt
 │    ├── repository/
 │    │    └── DiceRepository.kt
 │    └── usecase/
 │         └── RollDiceUseCase.kt
 ├── presentation/
 │    ├── DiceViewModel.kt
 │    └── ui/
 │         ├── DiceScreen.kt
 │         └── AnimatedDiceLottie.kt
 ├── di/
 │    └── AppModule.kt
 ├── MainActivity.kt
 └── README.md
```

---

## ⚙️ Dependencies

Tambahkan ke `build.gradle`:

```gradle
// Compose
implementation "androidx.compose.ui:ui:1.7.0"
implementation "androidx.compose.material3:material3:1.3.0"

// ViewModel + StateFlow
implementation "androidx.lifecycle:lifecycle-viewmodel-compose:2.8.3"

// Hilt
implementation "com.google.dagger:hilt-android:2.51.1"
kapt "com.google.dagger:hilt-compiler:2.51.1"

// Test
androidTestImplementation "androidx.compose.ui:ui-test-junit4:1.7.0"
androidTestImplementation "androidx.test.ext:junit:1.2.1"
```

---

## 🧱 Core Components

### 🎯 Domain Layer
```kotlin
data class DiceResult(val value: Int)
```

```kotlin
class RollDiceUseCase(private val repository: DiceRepository) {
    operator fun invoke(): DiceResult = repository.rollDice()
}
```

---

### 💾 Data Layer
```kotlin
class DiceRepositoryImpl : DiceRepository {
    override fun rollDice(): DiceResult {
        val value = Random.nextInt(6) + 1
        return DiceResult(value)
    }
}
```

---

### 🧠 Presentation Layer
```kotlin
class DiceViewModel(private val rollDiceUseCase: RollDiceUseCase) : ViewModel() {
    private val _diceResult = MutableStateFlow(DiceResult(1))
    val diceResult: StateFlow<DiceResult> = _diceResult

    fun rollDice() {
        _diceResult.value = rollDiceUseCase()
    }
}
```

---

### 🧩 Dependency Injection (Hilt)
```kotlin
@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Provides
    fun provideDiceRepository(): DiceRepository = DiceRepositoryImpl()

    @Provides
    fun provideRollDiceUseCase(repo: DiceRepository): RollDiceUseCase =
        RollDiceUseCase(repo)
}
```

---

## 🧪 Testing

### 🧠 Unit Test (UseCase)
```kotlin
@Test
fun `rollDice returns valid range`() {
    val repo = DiceRepositoryImpl()
    val useCase = RollDiceUseCase(repo)

    repeat(100) {
        val result = useCase()
        assertTrue(result.value in 1..6)
    }
}
```

---

### 🧩 Compose UI Test
```kotlin
@get:Rule
val composeRule = createComposeRule()

@Test
fun clickingRollDice_changesResult() {
    val viewModel = DiceViewModel(RollDiceUseCase(DiceRepositoryImpl()))
    composeRule.setContent { DiceScreen(viewModel) }

    composeRule.onNodeWithText("Roll Dice").performClick()
    composeRule.onNodeWithText("🎲 Result:").assertExists()
}
```

---

## 🧭 Running the App

```bash
./gradlew assembleDebug
```

Untuk menjalankan test:

```bash
./gradlew test
./gradlew connectedAndroidTest
```

---

## 📸 Screenshots

| Dice Roll | UI Test Success |
|------------|-----------------|
| ![ss1](docs/screenshot-1.png) | ![ss3](docs/screenshot-3.png) |

---

## 🌈 Highlights

- 🧱 **Clean Architecture** (Domain, Data, Presentation)
- 🧠 **MVVM + StateFlow** for reactive UI
- 🎨 **Jetpack Compose UI + Material3** design
- 🧩 **Hilt Dependency Injection**
- 🧪 **Unit & UI Testing** ready
- ⚙️ **Modern Gradle Version Catalog**
- 💯 **100% Kotlin**

---

## 🔗 Credits

- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Hilt](https://dagger.dev/hilt/)
- [Android Testing](https://developer.android.com/training/testing)

---
