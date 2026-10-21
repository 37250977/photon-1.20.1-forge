# libs/

本地依赖 jar 不纳入版本库（依赖为上一环端口工程的构建产物）。

构建前请把依赖放到这里，文件名必须与 `build.gradle` / `gradle.properties` 里的坐标逐字一致：

```
ldlib2-forge-1.20.1-2.2.41+forge.1.20.1-all.jar
kilagraph-forge-1.20.1-20.1.0.15.jar
```

来源：同批发布包里的 `dist/` 目录，或用上一环仓库自行 `gradlew jarJar` / `gradlew jar` 构建。