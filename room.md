### app/build.gradle.kts

```
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    id("com.google.devtools.ksp") version "2.2.10-2.0.2"
}
```

1. Sync Now
1. 直らなければ File → Invalidate Caches / Restart
1. 再度 Sync

### gradle.properties

```
android.disallowKotlinSourceSets=false
```

1. File > Sync Project with Gradle Files
1. Build > Clean Project
1. Build > Rebuild Project
