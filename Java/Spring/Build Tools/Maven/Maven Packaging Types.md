- The packaging type is important in any of Maven project
- Specifies the type of artifact the project produces
- Each packaging type follows a build lifecycle that consists of phases

## `pom`
- Helps to create **aggregators** and **parent projects**
- An aggregator OR multi-module project assembles sub-modules coming from different sources
	- These submodules are regular Maven projects and _follow their own build lifecycles_.
	- The aggregator POM has all the references of submodules under `<modules></modules>`
```xml
<modules>  
    <module>middleware</module>  
    <module>shared</module>  
    <module>backend</module>  
</modules>
```
- **A parent project allows you to define the inheritance relationship between POMs**
- The parent POM shares certain configurations, plugins, and dependencies, along with their versions. Most elements from the parent are inherited by its children

> Because there are no resources to process and no code to compile or test. Hence, the artifacts (moduels ) of pom projects generate themselves instead of any executable.



