JPA specifies seven optional lifecycle events that are called:

- before `persist` is called for a new entity – `@PrePersist` ‼️
	- the method annotated with `@PrePersist` runs just before the associated entity is first saved to the database
- after `persist` is called for a new entity – `@PostPersist`
- before an entity is removed – `@PreRemove`
- after an entity has been deleted – `@PostRemove`
- before the update operation – `@PreUpdate` ‼️
	- this method annotated with `@PreUpdate` will be triggered whenever the associated entity's database is updated
- after an entity is updated – `@PostUpdate`
- after an entity has been loaded – `@PostLoad`