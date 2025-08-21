# Architecture – The Favorites Super App

## 1. Overview

The app follows **Clean Architecture** principles:

- **UI Layer**: Compose screens, animations, navigation.
- **Domain Layer**: UseCases, business logic, validation.
- **Data Layer**: Repositories handling data from **Firestore**, **Room**, **Firebase Storage**.

Offline-first design ensures app works without internet; WorkManager handles background sync.

---

## 2. Layers

### 2.1 UI Layer

- Jetpack Compose for declarative UI
- Screens:
    - LoginScreen (Google Auth)
    - CategoryListScreen
    - ItemListScreen
    - ItemDetailScreen
- Handles user interaction, displays data from domain layer
- Animations and transitions implemented here

### 2.2 Domain Layer

- Contains **UseCases**:
    - `LoginUseCase`
    - `CreateCategoryUseCase`
    - `GetItemsUseCase`
    - `AddItemUseCase`
    - `SyncOfflineChangesUseCase`
- Pure business logic, independent of framework or storage

### 2.3 Data Layer

- **Repositories**:
    - `CategoryRepository`
    - `ItemRepository`
    - `AuthRepository`
- Handles:
    - Firestore queries
    - Room cache
    - Firebase Storage photo uploads
    - Offline sync with WorkManager
- Data sources abstracted → allows easy testing

---

## 3. Firebase Integration

- **Authentication**: Google login via Firebase Auth
- **Firestore**: stores categories and items
- **Storage**: stores item photos
- **WorkManager**: syncs offline changes with Firestore

---

## 4. Offline-first & Sync Flow

1. User adds/edits item offline → Room cache updated
2. WorkManager schedules sync task
3. Sync task pushes changes to Firestore
4. Conflict resolution: last-write-wins
5. UI updates automatically from LiveData / Flow

---

## 5. Security

- Firestore rules: only allow user-specific access
- Storage rules: secure file access by user
- AuthRepository handles token verification

---

## 6. Diagram Suggestions

- Use Case Diagram: login, category/item management, offline sync
- Class Diagram: repositories, usecases, data sources
- Sequence Diagram: add item offline → sync → Firestore
- Firebase Infra Diagram: Auth + Firestore + Storage + WorkManager
