# LAB_WEEK_08 - WorkManager & Foreground Service dengan Background Processing

## Link Google Drive
[Keseluruhan Project](https://drive.google.com/drive/u/5/folders/1DS-2ZPPu-BZnyLKs9uXhtsw7KDzx2QeM)

[Images & Screenshots](https://drive.google.com/drive/u/5/folders/1jMrIvuIsJeJNWEimv8mcpcQORuxIqXrU)

[APK File](https://drive.google.com/drive/u/5/folders/1T4qg5W-jmeTK4r_QwwXP3eF9-Bw3bhxr)


## Commit History
- **Commit No. 1** - add first & second worker
- **Commit No. 2** - add first foreground service  
- **Commit No. 3** - add third worker & second foreground service

## Fitur Aplikasi

### Background Processing dengan WorkManager
- **Sequential Worker Execution** - Menjalankan workers secara berurutan (FirstWorker → SecondWorker → ThirdWorker)
- **Network Constraints** - Workers hanya berjalan ketika device terhubung ke internet
- **Data Passing** - Mengirim dan menerima data antara Activity dan Workers
- **Thread Management** - Background processing di thread terpisah tanpa blocking UI

### Foreground Service dengan Notification
- **Dual Notification Services** - Dua service berbeda dengan channel notification terpisah
- **Real-time Countdown** - Notifikasi dengan countdown timer yang berbeda (7 detik dan 8 detik)
- **LiveData Observation** - Tracking completion service melalui LiveData
- **Foreground Service Type** - Service dengan tipe dataSync untuk background processing

### User Interface & Interaction
- **Toast Notifications** - Real-time feedback untuk setiap proses yang selesai
- **Permission Handling** - Auto-request notification permission untuk Android 13+
- **Sequential Execution** - Urutan proses yang jelas dan terstruktur

## Fitur Interaksi

### Worker Execution Flow
1. **FirstWorker** → 3 detik processing → "First process is done" toast
2. **SecondWorker** → 3 detik processing → "Second process is done" toast
3. **NotificationService** → 7 detik countdown → "Channel ID 001 process done" toast
4. **ThirdWorker** → 3 detik processing → "Third process is done" toast
5. **SecondNotificationService** → 8 detik countdown → "Channel ID 002 process done" toast

### Notification Features
- **Persistent Notification** - Notification yang tidak bisa di-dismiss user (ongoing)
- **Countdown Display** - Update real-time countdown di notification content
- **Click Action** - Redirect ke MainActivity ketika notification diklik
- **Channel Separation** - Dua channel berbeda dengan priority yang berbeda

## Teknologi yang Digunakan

### Android Architecture Components
- **WorkManager** - Untuk background task scheduling dan execution
- **LiveData** - Untuk reactive UI updates berdasarkan state changes
- **Foreground Service** - Untuk long-running background processes dengan notification

### Background Processing
- **OneTimeWorkRequest** - Single execution worker requests
- **WorkManager Constraints** - Network requirement constraints
- **HandlerThread** - Separate thread management untuk service operations
- **Thread.sleep()** - Simulasi heavy background processing

### Notification System
- **NotificationCompat** - Cross-version notification compatibility
- **NotificationChannel** - Channel management untuk Android O+
- **PendingIntent** - Deferred intent execution untuk notification clicks
- **Foreground Service** - Service dengan persistent system notification

### Permission & System
- **POST_NOTIFICATIONS** - Notification permission handling
- **FOREGROUND_SERVICE** - Permission untuk foreground service execution
- **ContextCompat** - Compatible context operations

## Struktur Data & Components

### Worker Classes
- **FirstWorker** - Background worker pertama dengan 3 detik processing
- **SecondWorker** - Background worker kedua dengan 3 detik processing  
- **ThirdWorker** - Background worker ketiga dengan 3 detik processing

### Service Classes
- **NotificationService** - Foreground service pertama dengan 7 detik countdown
- **SecondNotificationService** - Foreground service kedua dengan 8 detik countdown

### Data Flow
- **InputData** - Data passing dari Activity ke Workers menggunakan Data.Builder()
- **OutputData** - Result return dari Workers ke Activity
- **Intent Extras** - Data passing dari Activity ke Services
- **LiveData** - Completion tracking dari Services ke Activity

## Execution Chronology

### Phase 1: Worker Execution
```
0s: App Start
3s: FirstWorker Complete → Toast "First process is done"
6s: SecondWorker Complete → Toast "Second process is done"
```

### Phase 2: First Service Execution
```
6s: NotificationService Start → Notification appears (7s countdown)
13s: NotificationService Complete → Toast "Channel ID 001 process done"
```

### Phase 3: Final Worker & Service
```
13s: ThirdWorker Start
16s: ThirdWorker Complete → Toast "Third process is done"
16s: SecondNotificationService Start → Notification appears (8s countdown)
24s: SecondNotificationService Complete → Toast "Channel ID 002 process done"
```

## Key Features Implementation

### WorkManager Chain
```kotlin
workManager.beginWith(firstRequest)
    .then(secondRequest)
    .then(thirdRequest)
    .enqueue()
```

### Foreground Service Start
```kotlin
ContextCompat.startForegroundService(this, serviceIntent)
```

### Notification Channel Creation
```kotlin
val channel = NotificationChannel(channelId, channelName, importance)
```

### LiveData Observation
```kotlin
NotificationService.trackingCompletion.observe(this) { Id -> 
    showResult("Process for Notification Channel ID $Id is done!")
}
```
