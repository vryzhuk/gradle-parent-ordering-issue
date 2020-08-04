# Breadth-wise configuration ordering (parent first) is not working when `evaluationDependsOn` is used

Repo reproduces an issue on Gradle 6.5.1. It has next structure:

```
parent/
  - build.gradle -> subprojects {group='com.project.sub'}
  - child1/
    - build.gradle
main/
  - build.gradle -> evaluationDependsOn(':parent:child1')
build.gradle -> subprojects {group='com.project'}
settings.gradle -> include 'main', 'parent', 'parent:child1'
```

Result:
```
gradlew build
> Configure project :parent:child1  <-- should be done after :parent
child1 group is com.project
> Configure project :main
main group is com.project
> Configure project :parent
Configuring parent. Group: com.project
...
```
